Phần trước [[Xác thực người dùng từ xa bằng mã hoá đối xứng]]

# Keberos

Kerberos là dịch vụ xác thực được phát triển như một phần của Dự án Athena tại MIT. Vấn đề mà Kerberos chỉ ra là: Giả sử một môi trường phân tán mở trong đó người dùng tại các máy trạm mong muốn truy cập các dịch vụ trên các máy chủ được phân bổ trên toàn mạng. Ta muốn các máy chủ có thể hạn chế quyền truy cập đối với những người dùng được ủy quyền và có thể xác thực các yêu cầu dịch vụ. Trong môi trường này, không thể tin cậy một máy trạm để xác định chính xác người dùng của nó đối với các dịch vụ mạng. Đặc biệt, tồn tại ba mối đe dọa sau:

1. Một người dùng có thể có quyền truy cập vào một máy trạm cụ thể và giả vờ là một người dùng khác đang vận hành từ máy trạm đó.
2. Người dùng có thể thay đổi địa chỉ mạng của máy trạm để các yêu cầu được gửi từ máy trạm bị thay đổi có vẻ như đến từ máy trạm bị mạo danh.
3. Người dùng có thể nghe lén các trao đổi và sử dụng cuộc tấn công phát lại để giành quyền truy cập vào máy chủ hoặc làm gián đoạn hoạt động.

Trong bất kỳ trường hợp nào, người dùng trái phép có thể có quyền truy cập vào các dịch vụ và dữ liệu mà họ không được phép truy cập. Thay vì xây dựng một giao thức xác thực phức tạp tại mỗi máy chủ, Kerberos cung cấp một máy chủ xác thực tập trung có chức năng xác thực người dùng với máy chủ và xác thực máy chủ với người dùng. Không giống như hầu hết các cơ chế xác thực khác được mô tả trong cuốn sách này, Kerberos hoàn toàn dựa vào mã hóa đối xứng, không sử dụng mã hóa khóa công khai.

Hai phiên bản Kerberos được sử dụng phổ biến. Việc triển khai phiên bản 4 [MILL88, STEI88] vẫn tồn tại. Phiên bản 5 [KOHL94] khắc phục một số thiếu sót về bảo mật của phiên bản 4 và đã được ban hành dưới dạng Tiêu chuẩn Internet được đề xuất (RFC 4120 và RFC 4121).

Chúng ta bắt đầu phần này bằng một cuộc thảo luận ngắn gọn về động lực của phương pháp Kerberos. Sau đó, do tính phức tạp của Kerberos, tốt nhất nên bắt đầu bằng phần mô tả giao thức xác thực được sử dụng trong phiên bản 4. Điều này cho phép chúng ta thấy được bản chất của chiến lược Kerberos mà không cần xem xét một số chi tiết cần thiết để xử lý vấn đề bảo mật tinh vi. các mối đe dọa. Cuối cùng, chúng ta thực hành sử dụng phiên bản 5.

## Động lực

Nếu một nhóm người dùng được cung cấp máy tính cá nhân chuyên dụng không có kết nối mạng thì tài nguyên và tệp của người dùng có thể được bảo vệ bằng cách bảo mật vật lý cho từng máy tính cá nhân. Khi những người dùng này được phục vụ bởi một hệ thống chia sẻ thời gian tập trung, hệ điều hành chia sẻ thời gian phải cung cấp tính bảo mật. Hệ điều hành có thể thực thi các chính sách kiểm soát truy cập dựa trên danh tính người dùng và sử dụng quy trình đăng nhập để nhận dạng người dùng.

Ngày nay, cả hai kịch bản này đều không điển hình. Phổ biến hơn là kiến trúc phân tán bao gồm các máy trạm (máy khách) dành riêng cho người dùng và các máy chủ phân tán hoặc tập trung. Trong môi trường này, có thể hình dung ba cách tiếp cận bảo mật.

1. Dựa vào từng máy trạm khách riêng lẻ để đảm bảo danh tính của người dùng và dựa vào từng máy chủ để thực thi chính sách bảo mật dựa trên nhận dạng người dùng (ID).
2. Yêu cầu hệ thống máy khách tự xác thực với máy chủ nhưng phải tin tưởng vào hệ thống khách hàng liên quan đến danh tính của người sử dụng nó.
3. Yêu cầu người dùng chứng minh danh tính của mình đối với mỗi dịch vụ được yêu cầu. Đồng thời yêu cầu máy chủ chứng minh danh tính của họ với khách hàng.

Trong một môi trường nhỏ, khép kín trong đó tất cả các hệ thống được sở hữu và vận hành bởi một tổ chức duy nhất, chiến lược thứ nhất hoặc có lẽ thứ hai có thể là đủ. Nhưng trong một môi trường cởi mở hơn trong đó các kết nối mạng với các máy khác được hỗ trợ, cách tiếp cận thứ ba là cần thiết để bảo vệ thông tin người dùng và tài nguyên được lưu trữ trên máy chủ. Kerberos hỗ trợ cách tiếp cận thứ ba này. Kerberos giả định kiến trúc máy khách/máy chủ phân tán và sử dụng một hoặc nhiều máy chủ Kerberos để cung cấp dịch vụ xác thực.

Báo cáo được công bố đầu tiên về Kerberos [STEI88] đã liệt kê các yêu cầu sau.

- **Bảo mật**: Kẻ nghe lén mạng sẽ không thể lấy được thông tin cần thiết để mạo danh người dùng. Tổng quát hơn, Kerberos phải đủ mạnh để đối thủ tiềm năng không coi đó là mắt xích yếu.
- **Đáng tin cậy**: Đối với tất cả các dịch vụ dựa vào Kerberos để kiểm soát truy cập, việc thiếu dịch vụ Kerberos đồng nghĩa với việc thiếu các dịch vụ được hỗ trợ. Do đó, Kerberos phải có độ tin cậy cao và nên sử dụng kiến trúc máy chủ phân tán với một hệ thống có thể sao lưu hệ thống khác.
- **Minh bạch**: Lý tưởng nhất là người dùng không nên biết rằng việc xác thực đang diễn ra ngoài yêu cầu nhập mật khẩu.
- **Khả năng mở rộng**: Hệ thống phải có khả năng hỗ trợ số lượng lớn khách hàng và máy chủ. Điều này gợi ý một kiến trúc modular, phân tán.

Để hỗ trợ các yêu cầu này, sơ đồ tổng thể của Kerberos là sơ đồ dịch vụ xác thực bên thứ ba đáng tin cậy sử dụng giao thức dựa trên giao thức được đề xuất bởi Needham và Schroeder [NEED78], đã được thảo luận trong Phần 15.2. Nó được tin cậy theo nghĩa là máy khách và máy chủ tin cậy Kerberos làm trung gian xác thực lẫn nhau của chúng. Giả sử giao thức Kerberos được thiết kế tốt thì dịch vụ xác thực sẽ được bảo mật nếu bản thân máy chủ Kerberos được bảo mật.

## Kerberos Phiên bản 4

Phiên bản 4 của Kerberos sử dụng DES, trong một giao thức khá phức tạp, để cung cấp dịch vụ xác thực. Xem xét toàn bộ giao thức, rất khó để thấy sự cần thiết của nhiều yếu tố có trong đó. Do đó, chúng ta áp dụng chiến lược được Bill Bryant của Dự án Athena [BRYA88] sử dụng và xây dựng giao thức đầy đủ bằng cách trước tiên xem xét một số cuộc đối thoại giả định. Mỗi cuộc đối thoại liên tiếp sẽ tăng thêm độ phức tạp để chống lại các lỗ hổng bảo mật được tiết lộ trong cuộc đối thoại trước đó.

Sau khi kiểm tra giao thức, chúng ta xem xét một số khía cạnh khác của phiên bản 4.

### ĐỐI THOẠI XÁC THỰC ĐƠN GIẢN

**ĐỐI THOẠI XÁC THỰC ĐƠN GIẢN** Trong môi trường mạng không được bảo vệ, bất kỳ máy khách nào cũng có thể đăng ký dịch vụ với bất kỳ máy chủ nào. Rủi ro bảo mật có thể dễ dàng nhận thấy là mạo danh. Đối thủ có thể giả vờ là một khách hàng khác và có được các đặc quyền trái phép trên các máy chủ. Để chống lại mối đe dọa này, máy chủ phải có khả năng xác nhận danh tính của khách hàng yêu cầu dịch vụ. Mỗi máy chủ có thể được yêu cầu thực hiện nhiệm vụ này cho mỗi tương tác giữa máy khách/máy chủ, nhưng trong môi trường mở, điều này đặt ra gánh nặng đáng kể cho mỗi máy chủ.

Một cách khác là sử dụng máy chủ xác thực (AS - Authentication Server) biết mật khẩu của tất cả người dùng và lưu trữ chúng trong cơ sở dữ liệu tập trung. Ngoài ra, AS chia sẻ một khóa bí mật duy nhất với mỗi máy chủ. Các khóa này đã được phân phối vật lý hoặc theo một số cách an toàn khác. Hãy xem xét đoạn hội thoại giả định sau:

			(1) C -> AS: IDc || Pc || IDv
			(2) AS -> C: Ticket
			(3) C -> V: IDc || Ticket
			Ticket = E(Kv, [IDc || ADc || IDv])
	Trong đó:
		C = Máy khách
		AS = Máy chủ xác thực
		V = Máy chủ
		IDc = Định danh của người dùng tại C
		IDv = Định danh của V
		Pc = Mật khẩu của người dùng tại C
		ADc = Địa chỉ mạng của C
		Kv = Khoá bí mật được chia sẻ bởi AS và V

Trong trường hợp này, người dùng đăng nhập vào máy trạm và yêu cầu quyền truy cập vào máy chủ V. Mô-đun máy khách C trong máy trạm của người dùng yêu cầu mật khẩu của người dùng và sau đó gửi một tin nhắn đến AS bao gồm ID người dùng, ID của máy chủ và mật khẩu của người dùng. AS kiểm tra cơ sở dữ liệu của nó để xem liệu người dùng đã cung cấp mật khẩu thích hợp cho ID người dùng này hay chưa và liệu người dùng này có được phép truy cập vào máy chủ V hay không. Nếu cả hai lần kiểm tra đều vượt qua, AS chấp nhận người dùng là xác thực và bây giờ phải thuyết phục máy chủ rằng người dùng này là xác thực. Để làm như vậy, AS tạo một **vé** chứa ID người dùng, địa chỉ mạng và ID của máy chủ. Vé này được mã hóa bằng khóa bí mật được chia sẻ bởi AS và máy chủ này. Sau đó, vé này sẽ được gửi lại cho C. Vì vé đã được mã hóa nên C hoặc đối thủ không thể thay đổi được.

Với tấm vé này, C hiện có thể yêu cầu dịch vụ của V. C gửi tin nhắn đến V chứa ID của C và vé. V giải mã vé và xác minh rằng ID người dùng trong vé giống với ID người dùng không được mã hóa trong tin nhắn. Nếu hai điều này khớp nhau, máy chủ sẽ coi người dùng đã được xác thực và cấp dịch vụ được yêu cầu.

Mỗi thành phần của tin nhắn (3) đều có ý nghĩa quan trọng. Vé được mã hóa để ngăn chặn sự thay đổi hoặc giả mạo. ID của máy chủ (IDV) được bao gồm trong vé để máy chủ có thể xác minh rằng nó đã giải mã vé đúng cách. IDC được bao gồm trong vé để chỉ ra rằng vé này đã được phát hành thay mặt cho C. Cuối cùng, ADc có tác dụng chống lại mối đe dọa sau. Đối thủ có thể lấy được vé được truyền trong tin nhắn (2), sau đó sử dụng tên IDC và truyền tin nhắn có dạng (3) từ một máy trạm khác. Máy chủ sẽ nhận được một vé hợp lệ khớp với ID người dùng và cấp quyền truy cập cho người dùng trên máy trạm khác đó. Để ngăn chặn cuộc tấn công này, AS thêm vào vé địa chỉ mạng mà yêu cầu ban đầu được gửi đến. Bây giờ, vé chỉ hợp lệ nếu nó được truyền từ cùng một máy trạm đã yêu cầu vé ban đầu.

### ĐỐI THOẠI XÁC THỰC AN TOÀN HƠN

**ĐỐI THOẠI XÁC THỰC AN TOÀN HƠN** Mặc dù kịch bản trên giải quyết được một số vấn đề về xác thực trong môi trường mạng mở nhưng các vấn đề vẫn còn tồn tại. Hai đặc biệt nổi bật. Đầu tiên, chúng ta muốn giảm thiểu số lần người dùng phải nhập mật khẩu. Giả sử mỗi vé chỉ có thể được sử dụng một lần. Nếu người dùng C đăng nhập vào máy trạm vào buổi sáng và muốn kiểm tra thư của mình tại máy chủ thư thì C phải cung cấp mật khẩu để lấy vé cho máy chủ thư. Nếu C muốn kiểm tra thư nhiều lần trong ngày thì mỗi lần C đều yêu cầu nhập lại mật khẩu. Chúng tôi có thể cải thiện vấn đề bằng cách nói rằng vé có thể được tái sử dụng. Đối với một phiên đăng nhập duy nhất, máy trạm có thể lưu trữ vé máy chủ thư sau khi nhận được và sử dụng nó thay mặt người dùng để truy cập nhiều lần vào máy chủ thư.

Tuy nhiên, theo chương trình này, vẫn có trường hợp người dùng sẽ cần một vé mới cho mọi dịch vụ khác nhau. Nếu người dùng muốn truy cập máy chủ in, máy chủ thư, máy chủ tệp, v.v., phiên bản đầu tiên của mỗi lần truy cập sẽ yêu cầu một vé mới và do đó yêu cầu người dùng nhập mật khẩu.

Vấn đề thứ hai là kịch bản trước đó liên quan đến việc truyền văn bản gốc của mật khẩu [tin nhắn (1)]. Kẻ nghe trộm có thể lấy được mật khẩu và sử dụng bất kỳ dịch vụ nào mà nạn nhân có thể truy cập được.

Để giải quyết những vấn đề bổ sung này, chúng tôi giới thiệu một sơ đồ tránh mật khẩu văn bản thuần túy và một máy chủ mới, được gọi là **máy chủ cấp vé** (TGS - Ticket-granting server). Kịch bản mới (nhưng vẫn chỉ mang tính giả thuyết) như sau.

Một lần cho mỗi phiên đăng nhập của người dùng:

(1) C -> AS: IDc || ID(tgs)
(2) AS -> C: E(Kc, Ticket(tgs))

Một lần cho mỗi loại dịch vụ:

(3) C -> TGS: IDc || IDv || Ticket(tgs)
(4) TGS -> C: Ticket(v)

Một lần cho mỗi phiên dịch vụ:

(5) C -> V: IDc || Ticket(v)

![[Pasted image 20231023201342.png]]

Dịch vụ mới, TGS, phát hành vé cho người dùng đã được xác thực với AS. Do đó, trước tiên người dùng yêu cầu vé cấp vé (Tickettgs) từ AS. Mô-đun máy khách trong máy trạm của người dùng sẽ lưu phiếu này. Mỗi khi người dùng yêu cầu quyền truy cập vào một dịch vụ mới, máy khách sẽ đăng ký với TGS, sử dụng vé để xác thực chính nó. TGS sau đó cấp một vé cho dịch vụ cụ thể. Máy khách lưu từng vé cấp dịch vụ và sử dụng nó để xác thực người dùng của mình với máy chủ mỗi khi một dịch vụ cụ thể được yêu cầu. Chúng ta hãy xem chi tiết của sơ đồ này:

1. Máy khách thay mặt người dùng yêu cầu vé cấp vé bằng cách gửi ID người dùng của mình đến AS, cùng với TGS ID, cho biết yêu cầu sử dụng dịch vụ TGS.
2. AS phản hồi bằng một vé được mã hóa bằng khóa lấy từ mật khẩu của người dùng (Kc), khóa này đã được lưu trữ tại AS. Khi phản hồi này đến máy khách, máy khách sẽ nhắc người dùng nhập mật khẩu của mình, tạo khóa và cố gắng giải mã tin nhắn đến. Nếu cung cấp đúng mật khẩu, vé sẽ được khôi phục thành công.

Bởi vì chỉ có người dùng đúng mới biết mật khẩu, chỉ có người dùng đúng mới có thể lấy lại được vé. Vì vậy, chúng tôi đã sử dụng mật khẩu để lấy thông tin xác thực từ Kerberos mà không cần phải truyền mật khẩu ở dạng bản rõ. Bản thân vé bao gồm ID và địa chỉ mạng của người dùng và ID của TGS. Điều này tương ứng với kịch bản đầu tiên. Ý tưởng là khách hàng có thể sử dụng vé này để yêu cầu nhiều vé cấp dịch vụ. Vì vậy, vé cấp vé phải có khả năng tái sử dụng. Tuy nhiên, chúng tôi không mong muốn đối thủ có thể chiếm được tấm vé và sử dụng nó. Hãy xem xét tình huống sau: Đối thủ chiếm được vé đăng nhập và đợi cho đến khi người dùng đăng xuất khỏi máy trạm của mình. Sau đó, đối thủ sẽ có quyền truy cập vào máy trạm đó hoặc định cấu hình máy trạm của anh ta có cùng địa chỉ mạng với địa chỉ mạng của nạn nhân. Đối thủ có thể sử dụng lại vé để giả mạo TGS. Để chống lại điều này, vé bao gồm dấu thời gian, cho biết ngày và giờ vé được phát hành và thời gian tồn tại, cho biết khoảng thời gian mà vé có hiệu lực (ví dụ: 8 giờ). Do đó, khách hàng hiện có một vé có thể tái sử dụng và không cần phải làm phiền người dùng về mật khẩu cho mỗi yêu cầu dịch vụ mới. Cuối cùng, lưu ý rằng vé cấp vé được mã hóa bằng khóa bí mật chỉ AS và TGS mới biết. Điều này ngăn cản việc thay đổi vé. Vé được mã hóa lại bằng khóa dựa trên mật khẩu của người dùng. Điều này đảm bảo rằng vé chỉ có thể được phục hồi bởi đúng người dùng, cung cấp xác thực.

Bây giờ máy khách đã có vé cấp vé, họ có thể truy cập vào bất kỳ máy chủ nào với bước 3 và 4.

3. Máy khác thay mặt người dùng yêu cầu phiếu cấp dịch vụ. Với mục đích này, máy khách truyền một thông báo tới TGS chứa ID người dùng, ID của dịch vụ mong muốn và vé cấp vé.
4. TGS giải mã vé đến bằng cách sử dụng khóa chỉ được chia sẻ bởi AS và TGS (Ktgs) và xác minh sự thành công của việc giải mã bằng sự hiện diện của ID của nó. Nó kiểm tra để đảm bảo rằng thời gian tồn tại chưa hết hạn. Sau đó, nó so sánh ID người dùng và địa chỉ mạng với thông tin đến để xác thực người dùng. Nếu người dùng được phép truy cập vào máy chủ V, TGS sẽ cấp một vé để cấp quyền truy cập vào dịch vụ được yêu cầu.

Vé cấp dịch vụ có cấu trúc tương tự như vé cấp vé. Thật vậy, vì TGS là một máy chủ nên chúng ta mong đợi rằng các thành phần tương tự là cần thiết để xác thực một máy khách với TGS và để xác thực một máy khách với một máy chủ ứng dụng. Một lần nữa, vé chứa dấu thời gian và thời gian tồn tại. Nếu người dùng muốn truy cập vào cùng một dịch vụ sau đó, khách hàng có thể chỉ cần sử dụng vé cấp dịch vụ đã có trước đó và không cần phải làm phiền người dùng về mật khẩu. Lưu ý rằng vé được mã hóa bằng khóa bí mật (Kv) chỉ TGS và máy chủ mới biết, ngăn chặn sự thay đổi.

Cuối cùng, với một vé cấp dịch vụ cụ thể, khách hàng có thể truy cập vào dịch vụ tương ứng với bước 5.

5. Máy khách thay mặt người dùng yêu cầu quyền truy cập vào dịch vụ. Với mục đích này, máy khách sẽ truyền một thông báo đến máy chủ chứa ID người dùng và phiếu cấp dịch vụ. Máy chủ xác thực bằng cách sử dụng nội dung của vé.

Kịch bản mới này đáp ứng hai yêu cầu là chỉ một truy vấn mật khẩu cho mỗi phiên người dùng và bảo vệ mật khẩu người dùng.

### ĐỐI THOẠI XÁC THỰC PHIÊN BẢN 4

**ĐỐI THOẠI XÁC THỰC PHIÊN BẢN 4** Mặc dù kịch bản nêu trên tăng cường tính bảo mật so với lần thử đầu tiên nhưng vẫn còn hai vấn đề bổ sung. Trọng tâm của vấn đề đầu tiên là thời gian tồn tại gắn liền với vé cấp vé. Nếu thời gian tồn tại này rất ngắn (ví dụ: vài phút), thì người dùng sẽ được yêu cầu nhập mật khẩu nhiều lần. Nếu thời gian tồn tại dài (ví dụ: hàng giờ), thì đối thủ có nhiều cơ hội để phát lại hơn. Đối thủ có thể nghe trộm mạng và chụp một bản sao của vé cấp vé rồi đợi người dùng hợp pháp đăng xuất. Sau đó, đối thủ có thể giả mạo địa chỉ mạng của người dùng hợp pháp và gửi tin nhắn ở bước (3) tới TGS. Điều này sẽ cung cấp cho đối thủ quyền truy cập không giới hạn vào các tài nguyên và tệp có sẵn cho người dùng hợp pháp.

Tương tự, nếu đối thủ chiếm được vé cấp dịch vụ và sử dụng nó trước nó hết hạn, đối thủ có quyền truy cập vào dịch vụ tương ứng.

Vì vậy, chúng tôi đi đến một yêu cầu bổ sung. Dịch vụ mạng (TGS hoặc dịch vụ ứng dụng) phải có khả năng chứng minh rằng người sử dụng vé chính là người được cấp vé đó.

Vấn đề thứ hai là có trường hợp yêu cầu máy chủ phải tự xác thực cho người dùng. Nếu không có phương thức xác thực, đối thủ có thể phá hoại cấu hình để các tin nhắn đến máy chủ được chuyển hướng đến một vị trí khác. Khi đó, máy chủ giả sẽ có thể hoạt động như một máy chủ thật và thu thập mọi thông tin từ người dùng cũng như từ chối dịch vụ thực sự cho người dùng.

Chúng ta lần lượt xem xét những vấn đề này và tham khảo Bảng 15.1, cho thấy giao thức Kerberos thực tế. Hình 15.2 cung cấp một cái nhìn tổng quan được đơn giản hóa.

***Bảng 15.1** Tóm tắt về trao đổi tin nhắn Kerberos phiên bản 4*
![[Table_15_1.png]]

![[Figure_15_2.png]]
***Sơ đồ 15.2** Tổng quan về Kerberos*

Đầu tiên, hãy xem xét vấn đề vé cấp vé bị bắt và sự cần thiết phải xác định rằng người xuất trình vé cũng giống như máy khách được cấp vé. Mối đe dọa là đối thủ sẽ đánh cắp vé và sử dụng nó trước khi hết hạn. Để giải quyết vấn đề này, chúng ta hãy yêu cầu AS cung cấp cho cả máy khách và TGS một phần thông tin bí mật một cách an toàn. Sau đó, khách hàng có thể chứng minh danh tính của mình với TGS bằng cách tiết lộ thông tin bí mật một lần nữa theo cách an toàn. Một cách hiệu quả để thực hiện điều này là sử dụng khóa mã hóa làm thông tin bảo mật; đây được gọi là khóa phiên trong Kerberos.

Bảng 15.1a cho thấy kỹ thuật phân phối khóa phiên. Như trước đây, máy khách gửi một bản tin tới AS yêu cầu quyền truy cập vào TGS. AS phản hồi lại bằng một tin nhắn, được mã hóa bằng khóa lấy từ mật khẩu của người dùng (Kc), có chứa vé. Tin nhắn được mã hóa cũng chứa một bản sao của khóa phiên, K(c,tgs), trong đó các chỉ số dưới chỉ ra rằng đây là khóa phiên cho C và TGS. Vì khóa phiên này nằm bên trong tin nhắn được mã hóa bằng Kc nên chỉ có máy khách của người dùng mới có thể đọc được. Khóa phiên tương tự được bao gồm trong vé, chỉ TGS mới có thể đọc được. Do đó, khóa phiên đã được phân phối an toàn tới cả C và TGS.

Lưu ý rằng một số thông tin bổ sung đã được thêm vào giai đoạn đầu tiên của cuộc đối thoại. Tin nhắn (1) bao gồm dấu thời gian để AS biết rằng tin nhắn đó là kịp thời. Tin nhắn (2) bao gồm một số thành phần của vé ở dạng mà C có thể truy cập được. Điều này cho phép C xác nhận rằng vé này dành cho TGS và tìm hiểu thời gian hết hạn của nó.

Được trang bị vé và khóa phiên, C sẵn sàng tiếp cận TGS. Như trước đây, C gửi cho TGS một tin nhắn bao gồm vé cộng với ID của dịch vụ được yêu cầu [tin nhắn (3) trong Bảng 15.1b]. Ngoài ra, C truyền một trình xác thực, bao gồm ID và địa chỉ của người dùng C và dấu thời gian. Không giống như vé có thể tái sử dụng, trình xác thực chỉ được sử dụng một lần và có thời gian tồn tại rất ngắn. TGS có thể giải mã vé bằng khóa mà nó chia sẻ với AS. Vé này cho biết người dùng C đã được cung cấp khóa phiên K(c,tgs). Trên thực tế, phiếu ghi: “Bất kỳ ai sử dụng K(c,tgs) đều phải là C.” TGS sử dụng khóa phiên để giải mã trình xác thực. Sau đó, TGS có thể kiểm tra tên và địa chỉ từ bộ xác thực với tên và địa chỉ mạng của tin nhắn đến. Nếu tất cả đều trùng khớp thì TGS được đảm bảo rằng người gửi vé thực sự là chủ sở hữu thực sự của vé. Trên thực tế, người xác thực nói: “Tại thời điểm TS3, tôi sử dụng K(c,tgs).” Lưu ý rằng vé không chứng minh danh tính của bất kỳ ai nhưng là một cách để phân phối khóa một cách an toàn. Đây là trình xác thực chứng minh danh tính của máy khách. Bởi vì trình xác thực chỉ có thể được sử dụng một lần và có thời gian tồn tại ngắn nên nguy cơ đối thủ đánh cắp cả vé và trình xác thực để xuất trình sau này sẽ bị ngăn chặn.

Phản hồi từ TGS trong tin nhắn (4) tuân theo dạng tin nhắn (2). Tin nhắn được mã hóa bằng khóa phiên được chia sẻ bởi TGS và C và bao gồm khóa phiên được chia sẻ giữa C và máy chủ V, ID của V và dấu thời gian của vé. Bản thân vé bao gồm cùng một khóa phiên.

C hiện có một vé cấp dịch vụ có thể tái sử dụng cho V. Khi C xuất trình vé này, như trong tin nhắn (5), nó cũng gửi một trình xác thực. Máy chủ có thể giải mã vé, khôi phục khóa phiên và giải mã trình xác thực.

Nếu cần xác thực lẫn nhau, máy chủ có thể trả lời như trong tin nhắn (6) của Bảng 15.1. Máy chủ trả về giá trị của dấu thời gian từ trình xác thực, tăng dần lên 1 và được mã hóa trong khóa phiên. C có thể giải mã tin nhắn này để khôi phục dấu thời gian tăng dần. Vì tin nhắn đã được mã hóa bằng khóa phiên nên C được đảm bảo rằng nó chỉ có thể được tạo bởi V. Nội dung của tin nhắn đảm bảo với C rằng đây không phải là bản phát lại của một câu trả lời cũ.

Cuối cùng, khi kết thúc quá trình này, máy khách và máy chủ sẽ chia sẻ một khóa bí mật. Khóa này có thể được sử dụng để mã hóa các tin nhắn trong tương lai giữa hai bên hoặc để thay đổi khóa phiên ngẫu nhiên mới cho mục đích đó.

Hình 15.3 minh họa các trao đổi Kerberos giữa các bên. Bảng 15.2 tóm tắt sự điều chỉnh cho từng thành phần trong giao thức Kerberos.

![[Picture_15_3.png]]

**Hình 15.3** Trao đổi Kerberos

**Bảng 15.2** Cơ sở lý luận cho các thành phần của Giao thức Kerberos Phiên bản 4

| | |
|---|---|
|Tin nhắn (1)|Khách hàng yêu cầu vé cấp vé|
|IDc|Cho AS biết danh tính của người dùng từ máy khách này.|
|ID(tgs)|Báo cho AS biết người dùng yêu cầu quyền truy cập vào TGS|
|TS1|Cho phép AS xác minh rằng đồng hồ của máy khách đã được đồng bộ hóa với đồng hồ của AS.|
|Tin nhắn (2)|AS trả về vé cấp vé.|
|Kc|Mã hóa dựa trên mật khẩu của người dùng, cho phép AS và máy khách xác minh mật khẩu và bảo vệ nội dung tin nhắn (2).|
|K(c,tgs)|Bản sao khóa phiên mà máy khách có thể truy cập được tạo bởi AS để cho phép trao đổi an toàn giữa máy khách và TGS mà không yêu cầu họ chia sẻ khóa vĩnh viễn.|
|ID(tgs)|Xác nhận rằng vé này là dành cho TGS.|
|TS2|Thông báo cho khách hàng về thời gian vé này được phát hành.|
|Lifetime2|Thông báo cho khách hàng về thời hạn sử dụng của vé này.|
|Ticket(tgs)|Vé được khách hàng sử dụng để truy cập TGS.|
(a) Trao đổi dịch vụ xác thực

| | |
|---|---|
|Tin nhắn (3)|Khách hàng yêu cầu vé cấp dịch vụ.|
|IDv|Báo cho TGS rằng người dùng yêu cầu quyền truy cập vào máy chủ V.|
|Ticket(tgs)|Đảm bảo với TGS rằng người dùng này đã được xác thực bởi AS.|
|Authenticator(c)|Được tạo bởi khách hàng để xác thực vé.|
|Tin nhắn (4)|TGS trả về vé cấp dịch vụ.|
|K(c,tgs)|Khóa chỉ được chia sẻ bởi C và TGS để bảo vệ nội dung tin nhắn (4).|
|K(c,v)|Bản sao khóa phiên mà máy khách có thể truy cập được tạo bởi TGS để cho phép trao đổi an toàn giữa máy khách và máy chủ mà không yêu cầu họ chia sẻ khóa vĩnh viễn.|
|IDv|Xác nhận rằng vé này dành cho máy chủ V.|
|TS4|Thông báo cho khách hàng về thời gian vé này được phát hành|
|Ticket(v)|Vé được khách hàng sử dụng để truy cập máy chủ V.|
|Ticket(tgs)|Có thể tái sử dụng để người dùng không phải nhập lại mật khẩu.|
|K(tgs)|Vé được mã hóa bằng khóa chỉ AS và TGS mới biết để tránh giả mạo.|
|K(c,tgs)|Bản sao khóa phiên có thể truy cập được của TGS được sử dụng để giải mã trình xác thực, từ đó xác thực vé.|
|IDc|Cho biết chủ sở hữu hợp pháp của vé này.|
|ADc|Ngăn chặn việc sử dụng vé từ máy trạm không phải là máy trạm đã yêu cầu vé ban đầu.|
|ID(tgs)|Đảm bảo với máy chủ rằng vé đã được giải mã đúng cách.|
|TS2|Thông báo cho TGS về thời gian vé này được phát hành.|
|Lifetime2|Ngăn chặn phát lại sau khi vé hết hạn.|
|Authenticator(c)|Đảm bảo với TGS rằng người xuất trình vé giống như khách hàng được cấp vé có thời gian tồn tại rất ngắn để ngăn chặn việc lặp lại.|
|K(c,tgs)|Trình xác thực được mã hóa bằng khóa chỉ có khách hàng và TGS mới biết để tránh giả mạo.|
|IDc|Phải khớp ID trong vé để xác thực vé.|
|ADc|Phải khớp địa chỉ trong vé để xác thực vé.|
|TS3|Thông báo cho TGS về thời gian trình xác thực này được tạo.|
(b) Trao đổi dịch vụ cấp vé

| | |
|---|---|
|Tin nhắn (5)|Máy khách yêu cầu dịch vụ.|
|Ticket(v)|Đảm bảo với máy chủ rằng người dùng này đã được xác thực bởi AS.|
|Authenticator(c)|Được tạo bởi khách hàng để xác thực vé.|
|Tin nhắn (6)|Tùy chọn xác thực máy chủ với máy khách.|
|K(c,v)|Đảm bảo với C rằng tin nhắn này là từ V.|
|TS(5) + 1|Đảm bảo với C rằng đây không phải là sự lặp lại của một câu trả lời cũ.|
|Ticket(v)|Có thể tái sử dụng để khách hàng không cần yêu cầu vé mới từ TGS cho mỗi lần truy cập vào cùng một máy chủ.|
|Kv|Vé được mã hóa bằng khóa chỉ TGS và máy chủ mới biết để tránh giả mạo.|
|K(c,v)|Bản sao khóa phiên mà khách hàng có thể truy cập; được sử dụng để giải mã trình xác thực, từ đó xác thực vé.|
|IDc|Cho biết chủ sở hữu hợp pháp của vé này.|
|ADc|Ngăn chặn việc sử dụng vé từ máy trạm không phải là máy trạm đã yêu cầu vé ban đầu.|
|IDv|Đảm bảo với máy chủ rằng nó đã giải mã vé đúng cách.|
|TS4|Thông báo cho máy chủ thời gian vé này được phát hành.|
|Lifetime4|Ngăn chặn phát lại sau khi vé hết hạn.|
|Authenticator(c)|Đảm bảo với máy chủ rằng người xuất trình vé giống với máy khách đã phát hành vé; có thời gian tồn tại rất ngắn để ngăn chặn việc phát lại.|
|K(c,v)|Authenticator được mã hóa bằng khóa chỉ có máy khách và máy chủ mới biết để tránh giả mạo.|
|IDC|Phải khớp ID trong vé để xác thực vé.|
|ADC|Phải khớp địa chỉ trong vé để xác thực vé.|
|TS5|Thông báo cho máy chủ thời gian trình xác thực này được tạo.|
(c) Trao đổi xác thực máy khách/máy chủ

### VÙNG KERBEROS VÀ NHIỀU KERBERI

VÙNG KERBEROS VÀ NHIỀU KERBERI Một môi trường Kerberos đầy đủ dịch vụ bao gồm một máy chủ Kerberos, một số máy khách và một số máy chủ ứng dụng yêu cầu những điều sau:

1. Máy chủ Kerberos phải có ID người dùng và mật khẩu băm của tất cả người dùng tham gia trong cơ sở dữ liệu của nó. Tất cả người dùng đều được đăng ký với máy chủ Kerberos.
2. Máy chủ Kerberos phải chia sẻ khóa bí mật với mỗi máy chủ. Tất cả các máy chủ đều được đăng ký với máy chủ Kerberos.

Một môi trường như vậy được gọi là **vùng Kerberos**. Khái niệm **vùng** có thể được giải thích như sau. Vùng Kerberos là một tập hợp các nút được quản lý có chung cơ sở dữ liệu Kerberos. Cơ sở dữ liệu Kerberos nằm trên hệ thống máy tính chính Kerberos, hệ thống này cần được lưu giữ trong phòng an toàn về mặt vật lý. Bản sao chỉ đọc của cơ sở dữ liệu Kerberos cũng có thể nằm trên các hệ thống máy tính Kerberos khác. Tuy nhiên, mọi thay đổi đối với cơ sở dữ liệu phải được thực hiện trên hệ thống máy tính chủ. Việc thay đổi hoặc truy cập nội dung của cơ sở dữ liệu Kerberos yêu cầu mật khẩu chính Kerberos. Một khái niệm liên quan là khái niệm **Kerberos chính**, là một dịch vụ hoặc người dùng được hệ thống Kerberos biết đến. Mỗi Kerberos chính được xác định bằng tên chính của nó. Tên chính bao gồm ba phần: tên dịch vụ hoặc tên người dùng, tên cụ thể và tên vùng.

Mạng máy khách và máy chủ thuộc các tổ chức quản trị khác nhau thường tạo thành các vùng khác nhau. Nghĩa là, việc đăng ký người dùng và máy chủ trong một miền quản trị với máy chủ Kerberos ở nơi khác là không thực tế hoặc không phù hợp với chính sách quản trị. Tuy nhiên, người dùng trong một lĩnh vực có thể cần quyền truy cập vào máy chủ ở các vùng khác và một số máy chủ có thể sẵn sàng cung cấp dịch vụ cho người dùng từ các vùng khác, miễn là những người dùng đó được xác thực.

Kerberos cung cấp cơ chế hỗ trợ xác thực liên vùng như vậy. Để hai vùng hỗ trợ xác thực liên vùng, yêu cầu thứ ba được thêm vào:

3. Máy chủ Kerberos trong mỗi vùng tương tác chia sẻ một khóa bí mật với máy chủ ở vùng khác. Hai máy chủ Kerberos được đăng ký với nhau.

Lược đồ này yêu cầu máy chủ Kerberos ở một vùng tin tưởng máy chủ Kerberos ở vùng khác để xác thực người dùng. Hơn nữa, các máy chủ tham gia ở vùng thứ hai cũng phải sẵn sàng tin cậy máy chủ Kerberos ở vùng thứ nhất.

Với những quy tắc cơ bản này, chúng ta có thể mô tả cơ chế như sau (Hình 15.4): Người dùng mong muốn dịch vụ trên máy chủ ở một khu vực khác cần có vé cho máy chủ đó. Máy khách của người dùng làm theo các thủ tục thông thường để có quyền truy cập vào TGS cục bộ và sau đó yêu cầu vé cấp vé cho TGS từ xa (TGS ở một vùng khác). Sau đó, máy khách có thể đăng ký TGS từ xa để có được vé cấp dịch vụ cho máy chủ mong muốn trong phạm vi TGS từ xa.

Chi tiết về các sàn giao dịch được minh họa trong Hình 15.4 như sau (so sánh với Bảng 15.1).

![[Pasted image 20231027235900.png]]

![[Picture_15_4.png]]

**Hình 15.4** Yêu cầu dịch vụ ở một vùng khác

Vé được xuất trình tới máy chủ từ xa (Vrem) cho biết khu vực mà người dùng ban đầu được xác thực. Máy chủ chọn có thực hiện yêu cầu từ xa hay không.

Một vấn đề được đưa ra bởi cách tiếp cận nói trên là nó không có quy mô tốt ở nhiều vùng. Nếu có N vùng thì phải có N(N - 1)/2 trao đổi khóa an toàn để mỗi vùng Kerberos có thể tương tác với tất cả các vùng Kerberos khác.

## Kerberos phiên bản 5

Kerberos phiên bản 5 được chỉ ra trong RFC 4120 và cung cấp một số cải tiến so với phiên bản 4 [KOHL94]. Để bắt đầu, chúng tôi cung cấp cái nhìn tổng quan về những thay đổi từ phiên bản 4 sang phiên bản 5, sau đó xem giao thức phiên bản 5.

### SỰ KHÁC BIỆT GIỮA PHIÊN BẢN 4 VÀ 5

***SỰ KHÁC BIỆT GIỮA PHIÊN BẢN 4 VÀ 5*** Phiên bản 5 nhằm giải quyết những hạn chế của phiên bản 4 ở hai lĩnh vực: những thiếu sót về môi trường và những thiếu sót về kỹ thuật. Hãy để chúng tôi tóm tắt ngắn gọn những cải tiến trong từng lĩnh vực.

Kerberos phiên bản 4 được phát triển để sử dụng trong môi trường dự án Athena và do đó, không giải quyết đầy đủ nhu cầu phục vụ mục đích chung. Điều này dẫn đến **những thiếu sót về môi trường** sau đây.

1. **Sự phụ thuộc vào hệ thống mã hóa**: Phiên bản 4 yêu cầu sử dụng DES. Do đó, hạn chế chia sẻ đối với DES cũng như những nghi ngờ về độ an toàn của DES là điều đáng lo ngại. Trong phiên bản 5, văn bản mã hóa được gắn thẻ nhận dạng loại mã hóa để có thể sử dụng bất kỳ kỹ thuật mã hóa nào. Các khóa mã hóa được gắn thẻ với loại và độ dài, cho phép sử dụng cùng một khóa trong các thuật toán khác nhau và cho phép đặc tả các biến thể khác nhau trên một thuật toán nhất định.
2. **Phụ thuộc vào giao thức Internet**: Phiên bản 4 yêu cầu sử dụng địa chỉ Giao thức Internet (IP - Internet Protocol). Các loại địa chỉ khác, chẳng hạn như địa chỉ mạng ISO, không được cung cấp. Địa chỉ mạng phiên bản 5 được gắn thẻ loại và độ dài, cho phép sử dụng bất kỳ loại địa chỉ mạng nào.
3. **Thứ tự byte tin nhắn**: Trong phiên bản 4, người gửi tin nhắn sử dụng thứ tự byte do chính họ chọn và gắn thẻ cho tin nhắn để chỉ ra byte có ý nghĩa nhỏ nhất ở địa chỉ thấp nhất hoặc byte có ý nghĩa nhất ở địa chỉ thấp nhất. Kỹ thuật này hoạt động nhưng không tuân theo các quy ước đã được thiết lập. Trong phiên bản 5, tất cả cấu trúc thông báo được xác định bằng cách sử dụng Ký hiệu Cú pháp Trừu tượng Một (ASN.1 - Abstract Syntax Notation One) và Quy tắc Mã hóa Cơ bản (BER - Basic Encoding Rules), cung cấp thứ tự byte rõ ràng.
4. **Thời gian tồn tại của vé**: Giá trị thời gian tồn tại trong phiên bản 4 được mã hóa thành số lượng 8 bit theo đơn vị năm phút. Do đó, thời gian tồn tại tối đa có thể được biểu thị là 2^8 * 5 = 1280 phút (hơn 21 giờ một chút). Điều này có thể không phù hợp với một số ứng dụng (ví dụ: mô phỏng chạy trong thời gian dài yêu cầu thông tin xác thực Kerberos hợp lệ trong suốt quá trình thực thi). Trong phiên bản 5, vé bao gồm thời gian bắt đầu và thời gian kết thúc rõ ràng, cho phép vé có thời gian tồn tại tùy ý.
5. **Chuyển tiếp xác thực**: Phiên bản 4 không cho phép chuyển tiếp thông tin đăng nhập được cấp cho một máy khách đến một số máy chủ khác hoặc sử dụng bởi máy khách khác. Điều này sẽ có khả năng cho phép máy khách truy cập vào máy chủ và yêu cầu máy chủ đó truy cập vào máy chủ khác thay mặt cho máy khách. Ví dụ: một máy khách đưa ra yêu cầu tới máy chủ in, sau đó máy chủ này truy cập file của máy khách từ máy chủ file, sử dụng thông tin xác thực của máy khách để truy cập. Phiên bản 5 cung cấp khả năng này.
6. **Xác thực liên vùng**: Trong phiên bản 4, khả năng tương tác giữa N vùng yêu cầu số lượng N^2 mối quan hệ Kerberos-to-Kerberos, như được mô tả trước đó. Phiên bản 5 hỗ trợ một phương pháp yêu cầu ít mối quan hệ hơn, như đã được mô tả gần đây.

Ngoài những hạn chế về môi trường, còn có **những thiếu sót về mặt kỹ thuật** trong chính giao thức phiên bản 4. Hầu hết những thiếu sót này đã được ghi lại trong [BELL90] và phiên bản 5 cố gắng giải quyết những thiếu sót này. Những thiếu sót như sau.

1. **Mã hóa kép**: Lưu ý trong Bảng 15.1 [tin nhắn (2) và (4)] rằng các vé được cung cấp cho máy khách được mã hóa hai lần. Một lần bằng khóa bí mật của máy chủ đích và sau đó một lần nữa bằng khóa bí mật mà máy khách biết . Việc mã hóa thứ hai là không cần thiết và gây lãng phí về mặt tính toán.
2. **Mã hóa PCBC**: Mã hóa ở phiên bản 4 sử dụng chế độ không theo chuẩn của DES được gọi là **chuỗi khối mật mã lan truyền** (PCBC - propagating cipher block chaining). Người ta đã chứng minh rằng chế độ này dễ bị tấn công liên quan đến sự thay đổi lẫn nhau của các khối văn bản mã hóa [KOHL89]. PCBC nhằm mục đích cung cấp khả năng kiểm tra tính toàn vẹn như một phần của hoạt động mã hóa. Phiên bản 5 cung cấp các cơ chế toàn vẹn rõ ràng, cho phép sử dụng chế độ CBC tiêu chuẩn để mã hóa. Cụ thể, checksum hoặc mã băm được đính kèm vào tin nhắn trước khi mã hóa bằng CBC.
3. **Khóa phiên**: Mỗi vé bao gồm một khóa phiên được máy khách sử dụng để mã hóa trình xác thực được gửi đến dịch vụ được liên kết với vé đó. Ngoài ra, khóa phiên sau đó có thể được máy khách và máy chủ sử dụng để bảo vệ các tin nhắn được truyền trong phiên đó. Tuy nhiên, vì cùng một vé có thể được sử dụng nhiều lần để nhận dịch vụ từ một máy chủ cụ thể nên có nguy cơ đối thủ sẽ phát lại các tin nhắn từ phiên cũ tới máy khách hoặc máy chủ. Trong phiên bản 5, máy khách và máy chủ có thể thương lượng khóa phiên phụ, khóa này chỉ được sử dụng cho một kết nối đó. Một quyền truy cập mới của khách hàng sẽ dẫn đến việc sử dụng khóa phiên phụ mới.
4. **Tấn công mật khẩu**: Cả hai phiên bản đều dễ bị tấn công bằng mật khẩu. Tin nhắn từ AS đến máy khách bao gồm tài liệu được mã hóa bằng khóa dựa trên mật khẩu của máy khách. Kẻ tấn công có thể bắt được tin nhắn này và cố gắng giải mã nó bằng cách thử nhiều mật khẩu khác nhau. Nếu kết quả của quá trình giải mã thử nghiệm có dạng phù hợp thì đối thủ đã phát hiện ra mật khẩu của máy khách và sau đó có thể sử dụng mật khẩu đó để lấy thông tin xác thực từ Kerberos. Đây là kiểu tấn công mật khẩu tương tự được mô tả trong Chương 21, với các biện pháp đối phó tương tự được áp dụng. Phiên bản 5 cung cấp một cơ chế được gọi là xác thực trước, khiến cho việc tấn công mật khẩu trở nên khó khăn hơn nhưng không ngăn chặn được chúng.

### ĐỐI THOẠI XÁC THỰC PHIÊN BẢN 5

**ĐỐI THOẠI XÁC THỰC PHIÊN BẢN 5** Bảng 15.3 tóm tắt hội thoại cơ bản của phiên bản 5. Điều này được giải thích tốt nhất bằng cách so sánh với phiên bản 4 (Bảng 15.1).

Đầu tiên, hãy xem xét việc **trao đổi dịch vụ xác thực**. Tin nhắn (1) là yêu cầu của khách hàng cho vé cấp vé. Như trước đây, nó bao gồm ID của người dùng và TGS. Các yếu tố mới sau đây được thêm vào:

Bảng 15.3 Tóm tắt các trao đổi tin nhắn Kerberos phiên bản 5
![[Table_15_3.png]]

- **Realm**: Cho biết vùng của người dùng
- **Options**: Được sử dụng để yêu cầu đặt một số flag nhất định trong vé được trả lại
- **Times**: Được máy khách sử dụng để yêu cầu cài đặt thời gian sau trong yêu cầu:
	- **from**: thời gian bắt đầu yêu cầu cấp vé
	- **till**: thời gian hết hạn được yêu cầu đối với yêu cầu cấp vé 
	- **rtime**: yêu cầu gia hạn cho đến thời điểm
- **Nonce**: Giá trị ngẫu nhiên được lặp lại trong tin nhắn (2) để đảm bảo rằng phản hồi là mới và không bị đối thủ phát lại

Tin nhắn (2) trả về vé cấp vé, thông tin nhận dạng của máy khác và khối được mã hóa bằng khóa mã hóa dựa trên mật khẩu của người dùng. Khối này bao gồm khóa phiên được sử dụng giữa máy khách và TGS, thời gian được chỉ định trong tin nhắn (1), số nonce từ tin nhắn (1) và thông tin nhận dạng TGS. Bản thân vé bao gồm khóa phiên, thông tin nhận dạng cho máy khách, giá trị thời gian được yêu cầu và các flag phản ánh trạng thái của vé này cũng như các tùy chọn được yêu cầu. Những flag này giới thiệu chức năng mới quan trọng cho phiên bản 5. Hiện tại, chúng ta tạm hoãn nói về các flag này và tập trung vào cấu trúc tổng thể của giao thức phiên bản 5.

Bây giờ chúng ta hãy so sánh việc **trao đổi dịch vụ cấp vé** của phiên bản 4 và 5. Chúng ta thấy tin nhắn (3) cho cả hai phiên bản đều bao gồm trình xác thực, vé và tên của dịch vụ được yêu cầu. Ngoài ra, phiên bản 5 bao gồm thời gian được yêu cầu và các tùy chọn cho vé và một số nonce - tất cả đều có chức năng tương tự như chức năng của tin nhắn (1). Bản thân trình xác thực về cơ bản giống với trình xác thực được sử dụng trong phiên bản 4.

Tin nhắn (4) có cấu trúc tương tự như tin nhắn (2). Nó trả về một vé cộng với thông tin mà máy khách cần, với thông tin được mã hóa bằng khóa phiên hiện tại được chia sẻ giữa máy khách và TGS.

Cuối cùng, đối với việc **trao đổi xác thực máy khách/máy chủ**, một số tính năng mới xuất hiện trong phiên bản 5. Trong tin nhắn (5), máy khách có thể yêu cầu như một tùy chọn rằng cần phải có xác thực chéo. Trình xác thực bao gồm một số trường mới:

- **Khoá con**: Lựa chọn của khách hàng về khóa mã hóa sẽ được sử dụng để bảo vệ phiên ứng dụng cụ thể này. Nếu trường này bị bỏ qua, khóa phiên từ vé K(c,v) sẽ được sử dụng.
- **Số thứ tự**: Trường tùy chọn chỉ định số thứ tự bắt đầu được máy chủ sử dụng cho các tin nhắn được gửi đến máy khách trong phiên này. Tin nhắn có thể được đánh số thứ tự để phát hiện các bản phát lại

Nếu cần xác thực chéo, máy chủ sẽ phản hồi bằng tin nhắn (6). Tin nhắn này bao gồm dấu thời gian từ trình xác thực. Lưu ý rằng trong phiên bản 4, dấu thời gian đã tăng lên một. Điều này là không cần thiết trong phiên bản 5, vì bản chất của định dạng tin nhắn là đối phương không thể tạo tin nhắn (6) nếu không biết về các khóa mã hóa thích hợp. Trường khóa con, nếu có, sẽ ghi đè trường khóa con, nếu có, trong tin nhắn (5). Trường số thứ tự tùy chọn chỉ định số thứ tự bắt đầu được máy khách sử dụng.

### FLAG CỦA VÉ

**FLAG CỦA VÉ** Trường flag có trong vé ở phiên bản 5 hỗ trợ chức năng mở rộng so với chức năng có sẵn trong phiên bản 4. Bảng 15.4 tóm tắt các flag có thể được thêm vào trong vé.

**Bảng 15.4** Flag trong Kerberos phiên bản 5

| | |
|---|---|
|INITIAL|Vé này được phát hành bằng giao thức AS và không được phát hành dựa trên vé cấp vé.|
|PRE-AUTHENT|Trong quá trình xác thực ban đầu, khách hàng đã được KDC xác thực trước khi phát hành vé.|
|HW-AUTHENT|Giao thức được sử dụng để xác thực ban đầu yêu cầu sử dụng phần cứng dự kiến chỉ được sở hữu bởi khách hàng có tên.|
|RENEWABLE|Báo cho TGS rằng vé này có thể được sử dụng để lấy vé thay thế sẽ hết hạn vào một ngày sau đó.|
|MAY-POSTDATE|Báo với TGS rằng một vé được ghi ngày sau có thể được phát hành dựa trên vé cấp vé|
|POSTDATED|Cho biết rằng vé này đã được đăng lại; máy chủ cuối có thể kiểm tra trường thời gian xác thực để xem thời điểm xác thực ban đầu xảy ra.|
|INVALID|Vé này không hợp lệ và phải được KDC xác nhận trước khi sử dụng.|
|PROXIABLE|Thông báo cho TGS rằng một vé cấp dịch vụ mới có địa chỉ mạng khác có thể được phát hành dựa trên vé được xuất trình.|
|PROXY|Cho biết vé này là proxy.|
|FORWARDABLE|Báo cho TGS rằng một vé cấp vé mới có địa chỉ mạng khác có thể được phát hành dựa trên vé cấp vé này.|
|FORWARDED|Cho biết rằng vé này đã được chuyển tiếp hoặc được phát hành dựa trên xác thực liên quan đến vé cấp vé được chuyển tiếp.|

INITIAL flag chỉ ra rằng vé này được phát hành bởi AS chứ không phải bởi TGS. Khi một khách hàng yêu cầu một vé cấp dịch vụ từ TGS, nó sẽ xuất trình một vé cấp vé thu được từ AS. Trong phiên bản 4, đây là cách duy nhất để có được vé cấp dịch vụ. Phiên bản 5 cung cấp khả năng bổ sung để khách hàng có thể nhận được phiếu cấp dịch vụ trực tiếp từ AS. Tiện ích của việc này như sau: Một máy chủ, chẳng hạn như máy chủ thay đổi mật khẩu, có thể muốn biết rằng mật khẩu của khách hàng đã được kiểm tra gần đây.

PRE-AUTHENT flag, nếu được đặt, cho biết rằng khi AS nhận được yêu cầu ban đầu [tin nhắn (1)], nó đã xác thực máy khách trước khi phát hành vé. Hình thức chính xác của việc xác thực trước này vẫn chưa được chỉ định. Ví dụ: việc triển khai phiên bản 5 của MIT có tính năng xác thực trước dấu thời gian được mã hóa, được bật theo mặc định. Khi người dùng muốn nhận vé, nó phải gửi đến AS một khối xác thực trước chứa yếu tố gây nhiễu ngẫu nhiên, số phiên bản và dấu thời gian, tất cả đều được mã hóa trong khóa dựa trên mật khẩu của máy khách. AS giải mã khối và sẽ không gửi lại vé cấp vé trừ khi dấu thời gian trong khối xác thực trước nằm trong khoảng thời gian cho phép (khoảng thời gian để tính đến độ lệch đồng hồ và độ trễ mạng). Một khả năng khác là việc sử dụng thẻ thông minh để tạo ra các mật khẩu thay đổi liên tục có trong các tin nhắn được xác thực trước. Mật khẩu do thẻ tạo ra có thể dựa trên mật khẩu của người dùng nhưng được thẻ chuyển đổi để trên thực tế, mật khẩu tùy ý được sử dụng. Điều này ngăn chặn một cuộc tấn công dựa trên mật khẩu dễ đoán. Nếu thẻ thông minh hoặc thiết bị tương tự được sử dụng, điều này sẽ được biểu thị bằng flag HW-AUTHENT.

Khi một vé có thời hạn sử dụng lâu dài, có khả năng nó sẽ bị đối thủ đánh cắp và sử dụng trong một thời gian đáng kể. Nếu thời gian tồn tại ngắn được sử dụng để giảm bớt mối đe dọa thì chi phí liên quan đến việc tạo vé mới. Trong trường hợp cấp vé, máy khách sẽ phải lưu trữ khóa bí mật của người dùng, điều này rõ ràng có rủi ro hoặc liên tục yêu cầu người dùng nhập mật khẩu. Một kế hoạch thỏa hiệp là việc sử dụng vé tái tạo. Một vé có đặt flag RENEWABLE bao gồm hai thời gian hết hạn: Một cho vé cụ thể này và một là giá trị cho phép mới nhất trong thời gian hết hạn. Máy có thể gia hạn vé bằng cách xuất trình vé cho TGS với thời gian hết hạn mới được yêu cầu. Nếu thời gian mới nằm trong giới hạn giá trị cho phép mới nhất, TGS có thể phát hành một vé mới với thời gian phiên mới và thời gian hết hạn cụ thể muộn hơn. Ưu điểm của cơ chế này là TGS có thể từ chối gia hạn vé được thông báo là bị đánh cắp.

Khách hàng có thể yêu cầu AS cung cấp vé cấp vé với flag MAY-POSTDATE được đặt. Sau đó, khách hàng có thể sử dụng vé này để yêu cầu một vé được gắn cờ là POSTDATED và INVALID từ TGS. Sau đó, khách hàng có thể gửi vé đã đăng ký để xác thực. Lược đồ này có thể hữu ích khi chạy một công việc hàng loạt dài trên máy chủ yêu cầu vé theo định kỳ. Máy khách có thể nhận được một số vé cho phiên này cùng một lúc, với các giá trị thời gian trải đều. Tất cả trừ vé đầu tiên ban đầu đều không hợp lệ. Khi quá trình thực thi đạt đến thời điểm cần có vé mới, máy khách có thể xác thực vé phù hợp. Với cách tiếp cận này, khách hàng không phải sử dụng lại vé cấp dịch vụ của mình để có được vé cấp dịch vụ.

Trong phiên bản 5, máy chủ có thể hoạt động như một proxy thay mặt cho khách hàng, thực tế là chấp nhận thông tin xác thực và đặc quyền của khách hàng để yêu cầu dịch vụ từ máy chủ khác. Nếu máy khách muốn sử dụng cơ chế này, nó sẽ yêu cầu vé cấp vé có flag PROXIABLE được đặt. Khi vé này được đưa cho TGS, TGS được phép phát hành vé cấp dịch vụ với địa chỉ mạng khác; vé sau này sẽ có flag PROXY được đặt. Ứng dụng nhận được phiếu như vậy có thể chấp nhận hoặc yêu cầu xác thực bổ sung để cung cấp dấu vết kiểm tra.

Khái niệm proxy là một trường hợp hạn chế của thủ tục chuyển tiếp mạnh mẽ hơn. Nếu một vé được đặt flag FORWARDABLE, TGS có thể cấp cho người yêu cầu một vé cấp vé có địa chỉ mạng khác và được đặt cờ FORWARDED. Thẻ này sau đó có thể được đưa tới một TGS từ xa. Khả năng này cho phép máy khách có quyền truy cập vào máy chủ ở một vùng khác mà không yêu cầu mỗi Kerberos duy trì một khóa bí mật với các máy chủ Kerberos ở mọi vùng khác. Ví dụ, các vùng có thể được cấu trúc theo thứ bậc. Sau đó, khách hàng có thể đi lên cây đến một nút chung và sau đó quay lại để đến vùng mục tiêu. Mỗi bước đi sẽ liên quan đến việc chuyển tiếp một vé cấp vé tới TGS tiếp theo trên đường đi.
