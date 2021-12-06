# Kiến trúc Microservice

* Kiến trúc microservices bao gồm những service nhỏ, các service hoạt động độc lập và thường triển khai một công việc duy nhất.

<img src="https://docs.microsoft.com/en-us/azure/architecture/includes/images/microservices-logical.png">

### Microservices là gì?
* Microservices là một project nhỏ, độc lập và kết nối lỏng lẻo(loosely coupled) để một team nhỏ có thể code và duy trì.
* Mỗi một service có một codebase riêng biệt.
* Các service có khả năng triển khai độc lập, dễ dàng cập nhật, triển khai hoặc dựng lại toàn bộ ứng dụng.
* Các service chịu trách nhiệm duy trì dữ liệu của chính nó hoặc trạng thái bên ngoài. Điều này khác với mô hình truyền thống, nơi mà một lớp dữ liệu riêng biệt xử lý dữ liệu.
* Các service giao tiếp với nhau bằng cách sử dụng các API. Các triển khai chi tiết của mỗi service được ẩn với các service khác.
* Hỗ trợ lập trình đa ngôn ngữ. Các service không cần chia sẻ cùng thư viện hoặc framework, tức là trong một ứng dụng có thể có nhiều service với các ngôn hoặc framework khác nhau.

### Ngoài các service, trong kiến trúc microsercies còn có những thành phần đặc trưng khác.
  * Management/orchestration: Đây là thành phần chịu trách nhiệm đặt các service vào các node, xác định lỗi, cân bằng service giữa các node, ... Thông thường, thành phần này là một công nghệ có sẵn như Kubernetes chứ ko phải là một thứ chúng ta tự xây dựng.
    * Một Node là một máy worker trong Kubernetes và có thể là máy ảo hoặc máy vật lý, tuỳ thuộc vào cluster. https://kubernetes.io
  * API Gateway: API gateway là cổng truy cập cho client. Thay vì gọi trực tiếp service, client gọi API gateway sau đó API gateway sẽ gọi đến các service thích hợp.
    * Ưu điểm khi sử dụng API gateway:
      * Tách client khỏi service. Các service có thể nâng cấp lên phiên bản mới hoặc tái cấu trúc mà không cần cập nhật tât cả client.
      * Các service có thể sử dụng các giao tiếp protocols như là AMQP.
      * API gateway có thể thực hiện liên tục các chức năng như xác thực, ghi nhật kí, kết thúc SSL và cân bằng tải.

  
### Lợi ích khi sử dụng microservices
  * Agility:  Vì các service triển khai đôc lập nên có thể dễ dàng sửa lỗi và phát hành tính năng mới. Bạn có thể cập nhật service mà không cần triển khai lại toàn bộ ứng dụng và có thể khôi phục bản cập nhật trước đó nếu có lỗi xảy ra. Trong cách triển khai truyền thống, nếu có lỗi nó có thể chặn toàn bộ quá trình triển khai ứng dụng. Các tính năng mới có thể được giữ lại để chờ bản sửa lỗi được tích hợp, kiểm thử và phát hành.
  * Small, focused teams:  Một microservice phải đủ nhỏ để một team có thể xây dựng, kiểm thử và triển khai nó. Quy mô team nhỏ nên có thể phát triển nhanh hơn. Các team lớn thường kém năng suất hơn, vì giao tiếp chậm hơn, chi phí quản lý cao nên giảm đi sự nhanh nhẹn.
  * Small code base: Trong một ứng dụng monolithic, code ngày càng phụ thuộc vào nhau và trên nên rối hơn. Mỗi khi thêm tính năng mới sẽ phải sửa code ở nhiều nơi. Bằng cách không chia sẻ code hoặc data, kiến trúc microservices giảm thiểu sự phụ thuộc, giúp việc triển khai tính năng mới trở nên dễ dàng hơn.
  * Mix of technologies: Có thể chọn công nghệ phù hợp nhất hoặc sử dụng kết hợp nhiều công nghệ khác để triển khai một service;.
  * Fault isolation:  Nếu một microservice không hoạt động, nó cũng sẽ không làm gián đoạn ứng dụng, miễn là các microservice được thiết kế để xử lý lỗi một cách chính xác.
  * Scalability:  Các service có thể mở rộng một cách độc lập, cho phép bạn mở rộng các hệ thống con yêu cầu nhiều tài nguyên hơn mà không cần mở rộng toàn bộ ứng dụng. Sử dụng Kubernetes hoặc Service Fabric, bạn có thể đóng gói mật độ service cao hơn vào trong một máy chủ duy nhất, điều này cho phép sử dụng tài nguyên hiệu quả hơn.
  * Data isolation:  Cập nhật kho dữ liệu dễ dàng hơn vì chỉ có một service nhỏ bị ảnh hưởng. Trong ứng dụng monolithic, cập nhật dữ liệu rất khó khăn, vì các phần khác nhau của ứng dụng có thể đang dùng chung một kho dữ liệu, bất kì thay đổi nào đều có thể xảy ra rủi ro.
  
### Khó khăn
  * Complexity:  Một ứng dụng microservices có thể có nhiều thành phần hơn so với ứng dụng monolithic tương đương. Mỗi service thì đơn giản hơn nhưng toàn bộ hệ thống thì phức tạp hơn.
  * Development and testing: Việc viết một service nhỏ dựa trên các service phụ thuộc khác yêu cầu một cách tiếp cận khác so với monolithic hoặc layered application. Các công cụ hiện tại không phải lúc nào cũng được thiết kế để hoạt động với các phụ thuộc service. Việc tái cấu trúc qua lại giữa các service có thể khó khăn. Việc kiểm tra sự phụ thuộc của các service cũng là một thách thức, đặc biệt là khi ứng dụng đang phát triển nhanh chóng.
  * Lack of governace: Cách tiếp cận phi tập trung để xây dựng microservices có những ưu điểm, nhưng nó cũng có thể dẫn đến nhiều vấn đề. Nếu sử dụng nhiều ngôn ngữ và framework khác nhau có thể khiến ứng dụng trở nên khó duy trì.
  * Network congrestion and latency: Tắc nghẽn mạng và độ trễ. Việc sử dụng nhiều microservice có thể dẫn đến giao tiếp giữ các service nhiều hơn. Ngoài ra, nếu các service gọi qua lại lẫn nhau nhiều lần độ trễ bổ sung có thể trở thành một vấn đề. Bạn sẽ cần thiết kế API một cách cẩn thận
  * Data integrity: Mỗi microservice chịu trách nhiệm với dữ liệu của chính nó. Do đó, tính thống nhất của dữ liệu có thể là một thách thức.
  * Management: Để thành công với microservices đòi hỏi một nền tảng DevOps lớn. Việc log các hoạt động liên quan đến service có thể là một thách thức. Thông thường việc log các hoạt động phải gọi đến nhiều service cho một thao tác của người dùng.
  * Versioning: Các bản cập nhật service không được phá vỡ các service phụ thuộc vào nó. Nhiều service có thể cập nhật bất kỳ lúc nào, vì vậy nếu không thiết kế cẩn thận bạn có thể gặp vấn đề về khả năng tương thích ngược hoặc chuyển tiếp.
  * Skill set: Microservices là những hệ thống có tính phân tán cao. Phải đánh giá cẩn thận xem team bạn có đủ kỹ năng và kinh nghiệm để triển khai nó không.
### Best practices
  * Các service nên xoay quanh business domain.
  * Phân quyền mọi thứ. Các team chịu trách nhiệm thiết kế và xây dựng các service. Tránh chia sẻ code hoặc data.
  * Mỗi service dùng một kho dữ liệu. Sử dụng bộ nhớ tốt nhất cho từng service và kiểu dữ liệu.
  * Các service giao tiếp thông qua API. Tránh rò rỉ các triển khai chi tiết bên trong service.
  * Tránh ghép nối giữa các service.
  * Giảm tải các kết nối xuyên suốt như authentication và SSL cho port.
  * Giữ cho logic trong domain không có gateway. Gateway phải xử lý và định tuyến các yêu cầu của khách hàng mà không cần bất cứ logic nào từ domain. Nếu không, gateway sẽ trở nên phụ thuộc và có thể gây ra sự ghép nối giữa các service.
  * Các service nên có khớp nối lỏng lẻo và tính liên kết chức năng cao. Các tính năng có thể thay đổi cùng nhau nên được đóng gói và triển khai cùng nhau. Nếu chúng nằm trong các service riêng biệt, các service đó sẽ được liên kết chặt chẽ với nhau.
  * Cô lập lỗi. Sử dụng khả năng tự sửa lỗi để ngăn chặn sự cố trong một service.