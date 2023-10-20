Phần trước: [[Nguyên tắc xác thực người dùng từ xa]]

# Xác thực người dùng từ xa bằng mã hoá đối xứng

## Xác thực chéo

Như đã thảo luận trong Chương 14, hệ thống phân cấp hai cấp của các khóa mã hóa đối xứng có thể được sử dụng để cung cấp tính bảo mật cho hoạt động liên lạc trong môi trường phân tán. Nói chung, chiến lược này liên quan đến việc sử dụng một trung tâm phân phối khóa đáng tin cậy (KDC - Key Distribution Center). Mỗi bên trong mạng chia sẻ một khóa bí mật, được gọi là khóa chính, với KDC. KDC chịu trách nhiệm tạo các khóa để sử dụng trong thời gian ngắn qua kết nối giữa hai bên, được gọi là khóa phiên và phân phối các khóa đó bằng khóa chính để bảo vệ việc phân phối. Cách tiếp cận này khá phổ biến. Để làm ví dụ, chúng ta xem xét hệ thống Kerberos trong Phần 15.3. Cuộc thảo luận trong tiểu mục này có liên quan đến sự hiểu biết về cơ chế Kerberos.

Hình 14.3 minh họa một đề xuất ban đầu do Needham và Schroeder đưa ra để phân phối khóa bí mật sử dụng KDC, như đã đề cập trong Chương 14, bao gồm các tính năng xác thực. Giao thức có thể được tóm tắt như sau.

1. A -> KDC: IDa || IDb || N1
2. KDC -> A: E(Ka, [Ks || IDb || N1 || E(Kb, [Ks || IDa])])
3. A -> B: E(Kb, [Ks || IDa])
4. B -> A: E(Ks, N2)
5. A -> B: E(Ks, f(N2)) f() là hàm tổng quát để thay đổi giá trị của số dùng một lần

Khóa bí mật Ka và Kb lần lượt được chia sẻ giữa A với KDC và B với KDC. Mục đích của giao thức là phân phối một cách an toàn khóa phiên Ks cho A và B. Thực thể A thu được khóa phiên mới một cách an toàn ở bước 2. Tin nhắn ở bước 3 có thể được giải mã và do đó chỉ B mới hiểu được. Bước 4 phản ánh sự hiểu biết của B về Ks, và bước 5 đảm bảo B về sự hiểu biết của A về Ks và đảm bảo với B rằng đây là một thông điệp mới vì việc sử dụng số dùng một lần N2. Nhớ lại kiến thức ở Chương 14 rằng mục đích của bước 4 và 5 là ngăn chặn một kiểu tấn công phát lại nhất định. Đặc biệt, nếu đối thủ có thể nắm bắt được tin nhắn ở bước 3 và phát lại nó, điều này có thể làm gián đoạn các hoạt động ở B.

Mặc dù đã bắt tay ở bước 4 và 5, giao thức vẫn dễ bị tấn công bằng hình thức lặp lại. Giả sử đối thủ X có thể xâm phạm khóa phiên cũ. Phải thừa nhận rằng trường hợp này khó xảy ra hơn nhiều so với trường hợp đối thủ chỉ cần quan sát và ghi lại bước 3. Tuy nhiên, đây là một rủi ro bảo mật tiềm ẩn. X có thể mạo danh A và lừa B sử dụng khóa cũ bằng cách phát lại bước 3. Trừ khi B nhớ tất cả các khóa phiên trước đó được sử dụng với A, B sẽ không thể xác định rằng đây là một khóa phát lại. Nếu X có thể chặn tin nhắn bắt tay ở bước 4 thì nó có thể mạo danh phản hồi của A ở bước 5. Từ thời điểm này trở đi, X có thể gửi tin nhắn giả đến B có vẻ như đến từ A bằng khóa phiên đã được xác thực.

Denning đề xuất khắc phục điểm yếu này bằng cách sửa đổi giao thức Needham/Schroeder bao gồm việc bổ sung dấu thời gian vào bước 2 và 3. Đề xuất của cô ấy giả định rằng các khóa chính, Ka và Kb, là an toàn và nó bao gồm các bước sau.

1. A -> KDC: IDa || IDb 
2. KDC -> A: E(Ka, [Ks || IDb || T || E(Kb, [Ks || IDa || T])])
3. A -> B: E(Kb, [Ks || IDa || T])
4. B -> A: E(Ks, N1)
5. A -> B: E(Ks, f(N1))

T là dấu thời gian đảm bảo cho A và B rằng khóa phiên vừa được tạo. Do đó, cả A và B đều biết rằng việc phân phối khóa là một trao đổi vừa mới được thực hiện. A và B có thể xác minh tính kịp thời bằng cách kiểm tra `abs(Clock - T) < deltaT1 + deltaT2` trong đó deltaT1 là độ lệch thông thường ước tính giữa đồng hồ của KDC và đồng hồ cục bộ (tại A hoặc B) và deltaT2 là thời gian trễ mạng dự kiến. Mỗi nút có thể đặt đồng hồ của nó theo một số nguồn tham chiếu tiêu chuẩn. Bởi vì dấu thời gian T được mã hóa bằng các khóa chính an toàn nên đối thủ, ngay cả khi biết khóa phiên cũ, cũng không thể thành công vì việc lặp lại bước 3 sẽ bị B phát hiện là không kịp thời. Các bước 4 và 5 này xác nhận việc nhận khóa phiên tại B.

Giao thức Denning dường như cung cấp mức độ bảo mật cao hơn so với giao thức Needham/Schroeder. Tuy nhiên, một mối lo ngại mới được đặt ra: đó là sơ đồ mới này yêu cầu sự phụ thuộc vào các đồng hồ được đồng bộ hóa trên toàn mạng. Rủi ro dựa trên thực tế là các đồng hồ được phân phối có thể không được đồng bộ hóa do sự phá hoại hoặc lỗi trong đồng hồ hoặc cơ chế đồng bộ hóa. Sự cố xảy ra khi đồng hồ của người gửi đi trước đồng hồ dự định của người nhận. Trong trường hợp này, kẻ tấn công có thể chặn tin nhắn từ người gửi và phát lại nó sau khi dấu thời gian trong tin nhắn có hiệu lực tại trang web của người nhận. Việc phát lại này có thể gây ra kết quả không mong muốn. Gong gọi các cuộc tấn công như vậy là các cuộc **tấn công phát lại trễ**.

Một cách để chống lại các cuộc tấn công phát lại trễ là thực thi yêu cầu các bên thường xuyên kiểm tra đồng hồ của họ theo đồng hồ của KDC. Giải pháp thay thế khác, tránh được nhu cầu đồng bộ hóa đồng hồ, là dựa vào các giao thức bắt tay sử dụng những số dùng một lần. Phương án thứ hai này không dễ bị tấn công ngăn chặn phát lại, bởi vì các số dùng một lần mà người nhận sẽ chọn trong tương lai là điều không thể đoán trước được đối với người gửi. Giao thức Needham/Schroeder chỉ dựa vào số dùng một lần nhưng như chúng ta đã thấy, nó có những lỗ hổng khác.

Trong [KEHN92], một nỗ lực được thực hiện nhằm giải quyết những lo ngại về các cuộc tấn công phát lại trễ và đồng thời khắc phục các sự cố trong giao thức Needham/ Schroeder. Sau đó, sự không nhất quán trong giao thức sau này đã được ghi nhận và một chiến lược cải tiến đã được trình bày trong [NEUM93a]. Giao thức này là:

1. A -> B: IDa || Na
2. B -> KDC: IDb || Nb || E(Kb, [IDa || Na || Tb])
3. KDC -> A: E(Ka, [IDb || Na || Ks || Tb]) || E(Kb, [IDa || Ks || Tb]) || Nb
4. A -> B: E(Kb, [IDa || Ks || Tb]) || E(Ks, Nb)

Ta cùng phân tích từng bước một của cuộc trao đổi trên:

1. A bắt đầu trao đổi xác thực bằng cách tạo ra một số dùng một lần, Na, và gửi nó cùng với mã định danh của nó tới B ở dạng bản rõ. Số nonce này sẽ được trả lại cho A trong một tin nhắn được mã hóa cùng với khóa phiên, đảm bảo cho A về tính kịp thời của nó.
2. B thông báo cho KDC rằng cần có khóa phiên. Tin nhắn của B tới KDC bao gồm mã định danh của B và một số dùng một lần, Nb. Số dùng một lần này sẽ được trả lại cho B trong một tin nhắn được mã hóa bao gồm khóa phiên, đảm bảo cho B về tính kịp thời của nó. Thông điệp của B gửi đến KDC cũng bao gồm một khối được mã hóa bằng khóa bí mật được chia sẻ bởi B và KDC. Khối này được sử dụng để hướng dẫn KDC cấp thông tin xác thực cho A; khối chỉ định người nhận thông tin xác thực dự định, thời gian hết hạn được đề xuất cho thông tin xác thực và số dùng một lần nhận được từ A.
3. KDC chuyển tới A số dùng một lần của B và một khối được mã hóa bằng khóa bí mật mà B chia sẻ với KDC. Khối này đóng vai trò như một “tấm vé” có thể được A sử dụng cho các lần xác thực tiếp theo, như sẽ thấy. KDC cũng gửi cho A một khối được mã hóa bằng khóa bí mật được chia sẻ bởi A và KDC. Khối này xác minh rằng B đã nhận được tin nhắn ban đầu của A (IDb) và đây là tin nhắn kịp thời chứ không phải tin nhắn phát lại (Na), đồng thời nó cung cấp cho A khóa phiên (Ks) và giới hạn thời gian sử dụng nó (Tb).
4. A truyền vé cho B, cùng với số dùng một lần của B, sau đó được mã hóa bằng khóa phiên. Vé cung cấp cho B khóa bí mật được sử dụng để giải mã E(Ks, Nb) nhằm khôi phục số dùng một lần. Thực tế là số dùng một lần của B được mã hóa bằng khóa phiên xác thực rằng tin nhắn đến từ A và không phải là tin nhắn được phát lại.

Giao thức này cung cấp một phương tiện an toàn, hiệu quả để A và B thiết lập phiên bằng khóa phiên an toàn. Hơn nữa, giao thức để A sở hữu một khóa có thể được sử dụng cho lần xác thực tiếp theo với B, tránh việc phải liên hệ nhiều lần với máy chủ xác thực. Giả sử A và B thiết lập một phiên sử dụng giao thức nói trên và sau đó kết thúc phiên đó. Sau đó, trong thời hạn do giao thức thiết lập, A mong muốn một phiên mới với B. Giao thức sau đây diễn ra:

1. A -> B: E(Kb, [IDa || Ks || Tb]) || N'a
2. B -> A: N'b || E(Ks, N'a)
3. A -> B E(Ks, N'b)

Khi B nhận được tin nhắn ở bước 1, nó xác minh rằng vé chưa hết hạn. Số dùng một lần mới được tạo ra Na và Nb đảm bảo với mỗi bên rằng không có cuộc tấn công lặp lại.

Trong tất cả những điều đã nói ở trên, thời gian được chỉ định trong Tb là thời gian tương ứng với đồng hồ của B. Do đó, dấu thời gian này không yêu cầu đồng hồ đồng bộ vì B chỉ kiểm tra dấu thời gian tự tạo.

## Xác thực một chiều

Sử dụng mã hóa đối xứng, kịch bản phân phối khóa phi tập trung được minh họa trong Hình 14.5 là không thực tế. Lược đồ này yêu cầu người gửi gửi yêu cầu đến người nhận dự định, chờ phản hồi bao gồm khóa phiên và chỉ sau đó gửi tin nhắn.

Với một số cải tiến, chiến lược KDC được minh họa trong Hình 14.3 là một giải pháp phù hợp để mã hoá thư điện tử. Vì chúng tôi muốn tránh yêu cầu người nhận (B) trực tuyến cùng lúc với người gửi (A), nên phải loại bỏ bước 4 và 5. Đối với tin nhắn có nội dung M, trình tự như sau:

1. A -> KDC: IDa || IDb || N1
2. KDC -> A: E(Ka [Ks || IDb || N1 || E(Kb, [Ks || IDa])])
3. A -> B: E(Kb, [Ks || IDa]) || E(Ks, M)

Cách tiếp cận này đảm bảo rằng chỉ người nhận tin nhắn dự định mới có thể đọc được nó. Nó cũng cung cấp mức xác thực rằng người gửi là A. Như đã chỉ định, giao thức không bảo vệ chống lại việc phát lại. Một số biện pháp phòng vệ có thể được cung cấp bằng cách thêm dấu thời gian vào tin nhắn. Tuy nhiên, do có thể xảy ra sự chậm trễ trong quá trình xử lý email, những dấu thời gian như vậy có thể bị hạn chế tác dụng.

Phần tiếp theo [Kerberos](Kerberos)

