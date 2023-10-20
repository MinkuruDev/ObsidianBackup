# Nguyên tắc xác thực người dùng từ xa

Trong hầu hết các bối cảnh bảo mật máy tính, xác thực người dùng là khối xây dựng cơ bản và là tuyến phòng thủ chính. Xác thực người dùng là cơ sở cho hầu hết các loại kiểm soát truy cập và trách nhiệm giải trình của người dùng. RFC 4949 (Thuật ngữ bảo mật Internet) định nghĩa xác thực người dùng là quá trình xác minh danh tính được xác nhận bởi hoặc cho một thực thể hệ thống. Quá trình này bao gồm hai bước:

 - Bước nhận dạng: Trình bày một mã định danh cho hệ thống bảo mật. (Mã định danh phải được chỉ định cẩn thận vì danh tính được xác thực là cơ sở cho các dịch vụ bảo mật khác, chẳng hạn như dịch vụ kiểm soát truy cập.)
 - Bước xác minh: Trình bày hoặc tạo ra thông tin xác thực chứng thực sự ràng buộc giữa thực thể và mã định danh.

Ví dụ: người dùng Alice Toklas có thể có mã định danh người dùng ABTOKLAS. Thông tin này cần được lưu trữ trên những máy chủ hoặc hệ thống máy tính mà Alice muốn sử dụng và quản trị viên hệ thống cũng như những người dùng khác có thể biết.

Một mục thông tin xác thực điển hình liên quan đến ID người dùng này là mật khẩu, được giữ bí mật (chỉ Alice và hệ thống mới biết). Nếu không ai có thể lấy hoặc đoán được mật khẩu của Alice thì sự kết hợp giữa ID người dùng và mật khẩu của Alice sẽ cho phép quản trị viên thiết lập quyền truy cập của Alice và kiểm tra hoạt động của cô ấy. Vì ID của Alice không phải là bí mật nên người dùng hệ thống có thể gửi email cho cô ấy, nhưng vì mật khẩu của cô ấy là bí mật nên không ai có thể giả làm Alice.

Về bản chất, nhận dạng là phương tiện mà người dùng cung cấp danh tính được xác nhận cho hệ thống; xác thực người dùng là phương tiện để thiết lập tính hợp lệ của yêu cầu. Lưu ý rằng xác thực người dùng khác với xác thực tin nhắn.

Như được định nghĩa trong chương 12, xác thực tin nhắn là một thủ tục cho phép các bên giao tiếp xác minh rằng nội dung của tin nhắn nhận được không bị thay đổi và nguồn đó là xác thực. Chương này chỉ liên quan đến xác thực người dùng.

## Mô hình NIST để xác thực người dùng điện tử

NIST SP 800-63-2 (Hướng dẫn xác thực điện tử, tháng 8 năm 2013) định nghĩa xác thực người dùng điện tử là quá trình thiết lập sự tin cậy đối với danh tính người dùng được hiển thị dưới dạng điện tử cho hệ thống thông tin. Hệ thống có thể sử dụng danh tính được xác thực để xác định xem cá nhân được xác thực có được phép thực hiện các chức năng cụ thể hay không, chẳng hạn như giao dịch cơ sở dữ liệu hoặc quyền truy cập vào các nguồn tài nguyên của hệ thống. Trong nhiều trường hợp, việc xác thực và giao dịch hoặc chức năng được ủy quyền khác diễn ra trên một mạng mở như Internet. Việc xác thực tương tự và ủy quyền tiếp theo có thể diễn ra cục bộ, chẳng hạn như trên mạng cục bộ.

SP 800-63-2 xác định mô hình chung để xác thực người dùng bao gồm một số chủ thể và thủ tục. Chúng ta thảo luận về mô hình này dựa trên Hình 15.1.

Yêu cầu ban đầu để thực hiện xác thực người dùng là người dùng phải đăng ký với hệ thống. Sau đây là trình tự điển hình để đăng ký. Người nộp đơn nộp đơn lên **cơ quan đăng ký (RA - Registration Authority)** để trở thành **người đăng ký** của **nhà cung cấp dịch vụ thông tin xác thực (CSP - Credential Service Provider)**. Trong mô hình này, RA là một thực thể đáng tin cậy thiết lập và xác nhận danh tính của người nộp đơn đăng ký cho một CSP. CSP sau đó tham gia trao đổi với người đăng ký. Tùy thuộc vào chi tiết của hệ thống xác thực tổng thể, CSP cung cấp một số loại thông tin xác thực điện tử cho người đăng ký. Thông tin xác thực là cấu trúc dữ liệu liên kết chính xác danh tính và các thuộc tính bổ sung với token do người đăng ký sở hữu và có thể được xác minh khi được trình bày cho người xác minh trong giao dịch xác thực. Token có thể là khóa mã hóa hoặc mật khẩu được mã hóa để xác định người đăng ký. Token có thể được CSP phát hành, do người đăng ký trực tiếp tạo ra hoặc do bên thứ ba cung cấp. Token và thông tin xác thực có thể được sử dụng trong các sự kiện xác thực tiếp theo.

![picture 15.1](https://media.githubusercontent.com/media/MinkuruDev/ObsidianBackup/master/Cryptography/Assest/Picture_15_1.png)

*Hình 15.1: Mô hình kiến trúc xác thực điện tử NIST SP 800-63-2*

Khi người dùng được đăng ký làm người đăng ký, quá trình xác thực thực tế có thể diễn ra giữa người đăng ký và một hoặc nhiều hệ thống thực hiện xác thực và sau đó là ủy quyền. Bên được xác thực được gọi là **người yêu cầu** và bên xác minh danh tính đó được gọi là **người xác minh**. Khi người yêu cầu chứng minh được việc sở hữu và kiểm soát token cho người xác minh thông qua giao thức xác thực, người xác minh có thể xác minh rằng người yêu cầu là người đăng ký có tên trong thông tin xác thực tương ứng. Người xác minh chuyển xác nhận về danh tính của người đăng ký cho **bên tin cậy (RP - Relying Party)**. Xác nhận đó bao gồm thông tin nhận dạng về người đăng ký, chẳng hạn như tên người đăng ký, số nhận dạng được chỉ định khi đăng ký hoặc các thuộc tính khác của người đăng ký đã được xác minh trong quá trình đăng ký. Bên tin cậy có thể sử dụng thông tin được xác thực do người xác minh cung cấp để đưa ra quyết định cấp phép hoặc kiểm soát truy cập.

Một hệ thống được triển khai để xác thực sẽ khác hoặc phức tạp hơn mô hình đơn giản hóa này, nhưng mô hình này minh họa các vai trò và chức năng chính cần thiết cho một hệ thống xác thực an toàn.

## Phương pháp xác thực

Có bốn phương pháp cơ bản để xác thực danh tính người dùng, có thể được sử dụng riêng lẻ hoặc kết hợp với nhau:

- Điều gì đó mà cá nhân biết: Ví dụ bao gồm mật khẩu, số nhận dạng cá nhân (PIN) hoặc câu trả lời cho một bộ câu hỏi được sắp xếp trước.
- Thứ mà cá nhân sở hữu: Ví dụ bao gồm khóa mật mã, thẻ khóa điện tử, thẻ thông minh và khóa vật lý. Loại trình xác thực này được gọi là token.
- Bản chất của cá nhân (sinh trắc học tĩnh): Ví dụ bao gồm sự nhận dạng bằng dấu vân tay, võng mạc và khuôn mặt.
- Điều gì đó mà cá nhân thực hiện (sinh trắc học động): Các ví dụ bao gồm nhận dạng bằng mẫu giọng nói, đặc điểm chữ viết tay và nhịp gõ.

Tất cả các phương pháp này, nếu được triển khai và sử dụng đúng cách, có thể cung cấp xác thực người dùng an toàn. Tuy nhiên, phương pháp nào cũng có vấn đề. Kẻ thù có thể đoán hoặc đánh cắp mật khẩu. Tương tự, kẻ thù có thể giả mạo hoặc đánh cắp token. Người dùng có thể quên mật khẩu hoặc mất token. Hơn nữa, có một chi phí quản trị đáng kể để quản lý thông tin mật khẩu và token trên hệ thống cũng như bảo mật thông tin đó trên hệ thống. Về sinh trắc học người xác thực, có nhiều vấn đề khác nhau, bao gồm việc xử lý các kết quả dương tính giả và âm tính giả, sự chấp nhận của người dùng, chi phí và sự thuận tiện. Để xác thực người dùng dựa trên mạng, các phương pháp quan trọng nhất liên quan đến khóa mật mã và một số thông tin mà cá nhân đó biết, chẳng hạn như mật khẩu.

## Xác thực chéo

Một lĩnh vực ứng dụng quan trọng là các giao thức xác thực lẫn nhau. Các giao thức như vậy cho phép các bên giao tiếp xác thực chéo về nhận dạng của nhau và trao đổi khóa phiên. Chủ đề này đã được xem xét trong Chương 14. Ở đó, trọng tâm là phân phối khóa. Chúng ta quay lại chủ đề này ở đây để xem xét ý nghĩa rộng hơn của việc xác thực.

Trọng tâm của vấn đề trao đổi khóa xác thực là hai vấn đề: tính bảo mật và tính kịp thời. Để ngăn chặn việc giả mạo và ngăn chặn sự xâm phạm các khóa phiên, thông tin nhận dạng thiết yếu và khóa phiên phải được truyền đạt ở dạng mã hóa. Điều này đòi hỏi sự tồn tại trước đó của khóa bí mật hoặc khóa công khai có thể được sử dụng cho mục đích này. Vấn đề thứ hai, tính kịp thời, rất quan trọng vì nguy cơ phát lại tin nhắn. Những lần phát lại như vậy, trong trường hợp xấu nhất, có thể cho phép đối thủ xâm phạm khóa phiên hoặc mạo danh thành công một bên khác. Ở mức tối thiểu, việc phát lại thành công có thể làm gián đoạn hoạt động bằng cách đưa ra cho các bên những thông báo có vẻ chân thực nhưng thực tế không phải vậy.

Các ví dụ về **tấn công lặp lại**:

1. Cuộc tấn công lặp lại đơn giản nhất là cuộc tấn công trong đó đối thủ chỉ cần sao chép một thông điệp và phát lại nó sau đó.
2. Đối thủ có thể phát lại tin nhắn có dấu thời gian trong khoảng thời gian hợp lệ. Nếu cả bản gốc và bản phát lại đều đến trong khoảng thời gian đó thì sự cố này có thể được ghi lại.
3. Như với ví dụ (2), đối phương có thể phát lại tin nhắn được đánh dấu thời gian trong khoảng thời gian hợp lệ, nhưng ngoài ra, đối phương sẽ chặn tin nhắn gốc. Vì vậy, sự lặp lại không thể được phát hiện.
4. Một cuộc tấn công khác liên quan đến việc phát lại ngược mà không sửa đổi. Đây là phát lại cho người gửi tin nhắn. Cuộc tấn công này có thể xảy ra nếu sử dụng mã hóa đối xứng và người gửi không thể dễ dàng nhận ra sự khác biệt giữa tin nhắn được gửi và tin nhắn nhận được dựa trên nội dung.

Một cách tiếp cận để đối phó với các cuộc tấn công lặp lại là đính kèm số thứ tự vào mỗi tin nhắn được sử dụng trong trao đổi xác thực. Một tin nhắn mới chỉ được chấp nhận nếu số thứ tự của nó đúng thứ tự. Sự khó khăn của cách tiếp cận này là nó yêu cầu mỗi bên phải theo dõi số thứ tự cuối cùng của bên khác mà họ đã thực hiện. Do nhược điểm này nên số thứ tự thường không được sử dụng để xác thực và trao đổi khóa. Thay vào đó, một trong hai cách tiếp cận phổ biến sau đây được sử dụng:

- **Dấu thời gian**: Bên A chỉ chấp nhận tin nhắn là mới nếu tin nhắn chứa **dấu thời gian** mà theo đánh giá của A là đủ gần với hiểu biết của A về thời gian hiện tại. Cách tiếp cận này yêu cầu đồng hồ giữa những người tham gia khác nhau phải được đồng bộ hóa.
- **Thử thách/phản hồi**: Bên A, chờ đợi một tin nhắn mới từ B, trước tiên gửi cho B một **số dùng một lần** (thử thách) và yêu cầu tin nhắn (phản hồi) tiếp theo nhận được từ B phải chứa giá trị số dùng một lần chính xác.

Có thể lập luận rằng không nên sử dụng phương pháp dấu thời gian cho các ứng dụng hướng kết nối vì những khó khăn cố hữu của kỹ thuật này. Đầu tiên, cần có một số loại giao thức để duy trì sự đồng bộ hóa giữa các đồng hồ bộ xử lý khác nhau. Giao thức này phải vừa có khả năng chịu lỗi để đối phó với các lỗi mạng, vừa phải an toàn để đối phó với các cuộc tấn công thù địch. Thứ hai, cơ hội cho một cuộc tấn công thành công sẽ xuất hiện nếu có sự mất đồng bộ tạm thời do lỗi trong cơ chế đồng hồ của một trong các bên. Cuối cùng, do tính chất thay đổi và không thể đoán trước của độ trễ mạng, nên không thể mong đợi các đồng hồ phân tán sẽ duy trì sự đồng bộ hóa chính xác. Do đó, bất kỳ quy trình dựa trên dấu thời gian nào cũng phải cho phép một khoảng thời gian đủ lớn để đáp ứng độ trễ của mạng nhưng cũng đủ nhỏ để giảm thiểu cơ hội bị tấn công.

Mặt khác, cách tiếp cận thách thức-phản hồi không phù hợp với loại ứng dụng không kết nối, bởi vì nó đòi hỏi chi phí bắt tay trước bất kỳ quá trình truyền không kết nối nào, phủ nhận một cách hiệu quả đặc điểm chính của giao dịch không kết nối. Đối với các ứng dụng như vậy, việc dựa vào một số loại máy chủ thời gian an toàn và nỗ lực nhất quán của mỗi bên để giữ cho đồng hồ của mình được đồng bộ hóa có thể là cách tiếp cận tốt nhất.

## Xác thực một chiều

Một ứng dụng mà mã hóa ngày càng phổ biến là thư điện tử (email). Bản chất của thư điện tử và lợi ích chính của nó là người gửi và người nhận không nhất thiết phải trực tuyến cùng một lúc. Thay vào đó, thư email sẽ được chuyển tiếp đến hộp thư điện tử của người nhận, nơi nó được lưu vào bộ đệm cho đến khi người nhận sẵn sàng đọc nó.

“Phong bì” hoặc tiêu đề của thư email phải rõ ràng để thư có thể được xử lý bằng giao thức email lưu trữ và chuyển tiếp, chẳng hạn như Giao thức truyền thư đơn giản (SMTP) hoặc X.400. Tuy nhiên, điều mong muốn là giao thức xử lý thư không yêu cầu quyền truy cập vào dạng văn bản gốc của thư, vì điều đó đòi hỏi phải tin cậy vào cơ chế xử lý thư. Theo đó, thư email phải được mã hóa sao cho hệ thống xử lý thư không sở hữu khóa để có thể giải mã.

Yêu cầu thứ hai là **xác thực**. Thông thường, người nhận muốn có sự đảm bảo nào đó rằng tin nhắn đó đến từ nguồn được cho là người gửi.

Phần tiếp theo: [Xác thực người dùng từ xa bằng mã hoá đối xứng](./Xác%20thực%20người%20dùng%20từ%20xa%20bằng%20mã%20hoá%20đối%20xứng.md)