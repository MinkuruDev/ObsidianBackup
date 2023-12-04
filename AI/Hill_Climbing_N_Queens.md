# Lý do chọn đề tài

# I. Giới thiệu về thuật toán leo đồi

## 1.1 Tổng quan về thuật toán leo đồi

Thuật toán tìm kiếm leo đồi là một phương pháp tìm kiếm cục bộ được sử dụng trong các bài toán tối ưu hóa. Điều đặc biệt về thuật toán này là nó tập trung vào việc tìm kiếm trong các khu vực lân cận của không gian trạng thái, chứ không phải duyệt qua toàn bộ không gian trạng thái.

Thuật toán tìm kiếm leo đồi thuộc loại thuật toán tìm kiếm giải pháp tối ưu, với nguyên lý hoạt động dựa trên việc kiểm tra và di chuyển giữa các trạng thái. Mỗi bước trong quá trình tìm kiếm, thuật toán xem xét trạng thái hiện tại và khám phá các trạng thái láng giềng để tìm ra trạng thái có hàm mục tiêu tốt hơn.

Quá trình tìm kiếm dừng lại khi thuật toán đạt tới một đỉnh của đồ thị hàm mục tiêu, nơi không có trạng thái láng giềng nào có giá trị mục tiêu lớn hơn. Điều này ám chỉ rằng thuật toán đã đạt được một giải pháp tối ưu tại điểm hiện tại.

Một đặc điểm quan trọng của tìm kiếm leo đồi là không lưu lại những trạng thái đã qua. Điều này giúp giảm bớt không gian bộ nhớ được sử dụng và tập trung vào việc tìm kiếm trong lân cận của trạng thái hiện tại. Hơn nữa, thuật toán không nhìn xa hơn hàng xóm của trạng thái hiện thời, chỉ tập trung vào việc cải thiện từng bước di chuyển một cách tối ưu.

Tuy tìm kiếm leo đồi có thể bị mắc kẹt trong các điểm cực đại địa phương, nhưng với một số cải tiến nhất định, nó vẫn là một phương pháp linh hoạt và hiệu quả cho nhiều bài toán tối ưu hóa.

## 1.2 Đặc điểm của thuật toán leo đồi

### 1.2.1 Là một thuật toán tìm kiếm cục bộ

Là một thuật toán tìm kiếm cục bộ, tìm kiếm leo đồi tập trung vào việc cải thiện giải pháp từ một trạng thái hiện tại thông qua việc khám phá các trạng thái láng giềng. Đặc điểm quan trọng của thuật toán này là khả năng di chuyển từ một trạng thái sang trạng thái khác mà không yêu cầu duyệt toàn bộ không gian trạng thái, giúp giảm bớt độ phức tạp của thuật toán.

Thuật toán tìm kiếm leo đồi thường bắt đầu với một giải pháp bất kỳ và cố gắng cải thiện nó liên tục bằng cách thực hiện các bước di chuyển nhỏ trong không gian trạng thái. Điều này thường liên quan đến việc thay đổi giá trị của một hoặc vài biến trong giải pháp hiện tại và kiểm tra xem có cải thiện giá trị mục tiêu hay không. Nếu có, giải pháp mới được chấp nhận và trở thành trạng thái hiện tại, và quá trình tìm kiếm tiếp tục từ đó.

Vì chỉ quan tâm đến các trạng thái lân cận và không lưu lại lịch sử tìm kiếm, thuật toán này thường rất hiệu quả với bộ nhớ và thời gian. Tuy nhiên, điều này cũng đồng nghĩa với việc có thể mắc kẹt ở các điểm cực đại địa phương và không đảm bảo tìm được lời giải tối ưu toàn cục.

Trong quá trình tìm kiếm, thuật toán không quan tâm đến lịch sử tìm kiếm hay đường đi đã đi qua. Nó chỉ tập trung vào việc cải thiện từng bước di chuyển, có thể bỏ qua các trạng thái không cung cấp cơ hội cải thiện mục tiêu.

### 1.2.2 Thực hiện một tìm kiếm heuristic (đánh giá)

Một hàm heuristic là một hàm đánh giá tất cả các lựa chọn tiềm năng trong thuật toán tìm kiếm dựa trên thông tin có sẵn giúp thuật toán lựa chọn tuyến đường tốt nhất đến lời giải
Đó đơn giản chỉ là một ước lượng về khả năng dẫn đến lời giải tính từ trạng thái đó (khoảng cách giữa trạng thái hiện tại và trạng thái đích)

Thuật toán tìm kiếm leo đồi thực hiện một tìm kiếm heuristic, nghĩa là nó sử dụng các thông tin phụ trợ để đánh giá giá trị của các trạng thái và hướng dẫn quá trình tìm kiếm. Heuristic là một phương pháp ước lượng giá trị mục tiêu mà không cần kiểm tra tất cả các trạng thái.

Trong ngữ cảnh của tìm kiếm leo đồi, heuristic thường được sử dụng để đánh giá chất lượng của một trạng thái và ước lượng xem nó có tiếp cận được gần lời giải tối ưu hay không. Heuristic giúp thuật toán quyết định xem nên di chuyển từ trạng thái hiện tại đến trạng thái láng giềng nào có vẻ triển vọng hơn.

Một số heuristic phổ biến bao gồm ước lượng khoảng cách từ trạng thái hiện tại đến trạng thái đích (heuristic h), ước lượng chi phí để di chuyển từ trạng thái hiện tại đến trạng thái đích (heuristic g), hoặc kết hợp cả hai để có heuristic tổng cộng (heuristic f).

Quan trọng nhất, heuristic giúp tìm kiếm leo đồi chủ động hơn trong việc chọn lựa trạng thái tiếp theo để kiểm tra, thay vì duyệt qua mọi trạng thái. Heuristic là một công cụ mạnh mẽ giúp giảm bớt độ phức tạp của thuật toán, đặc biệt là trong các bài toán có không gian trạng thái lớn.

Tuy nhiên, sự hiệu quả của heuristic cũng phụ thuộc vào chất lượng và tính chất của heuristic cụ thể được sử dụng. Một heuristic tốt cung cấp thông tin chính xác và hữu ích về hướng di chuyển tốt nhất, trong khi một heuristic kém có thể dẫn đến việc lạc quẻ hoặc mắc kẹt trong điểm cực đại địa phương.

### 1.2.3 Là một biến thể của thuật toán tạo và kiểm tra (generate-and-test)

Các bước của thuật toán:

- Bước 1: Tạo ra các giải pháp có thể.
- Bước 2: Đánh giá để xem liệu đây có phải là lời giải mong muốn.
- Bước 3: Nếu lời giải đã được tìm thấy, kết thúc; nếu không, quay lại Bước 1.

Thuật toán leo đồi sử dụng phản hồi từ thủ tục kiểm tra và bộ tạo ra để quyết định bước tiếp theo trong không gian tìm kiếm. Chính vì vậy, chúng ta gọi nó là một biến thể của thuật toán tạo và kiểm tra (generate-and-test algorithm).

Tìm kiếm leo đồi có thể xem là một biến thể của thuật toán tạo và kiểm tra (generate-and-test) trong ngữ cảnh tìm kiếm giải pháp. Trong thuật toán tạo và kiểm tra truyền thống, ta tạo ra các giải pháp ứng viên và kiểm tra chúng để xem liệu chúng có là giải pháp mong muốn hay không. Ngược lại, tìm kiếm leo đồi chủ yếu tập trung vào việc tìm kiếm trong không gian trạng thái và cố gắng cải thiện một giải pháp hiện tại.

Trong quá trình tạo và kiểm tra, ta thường tạo ra một tập hợp các giải pháp ngẫu nhiên hoặc theo một quy luật nào đó và sau đó kiểm tra từng giải pháp để xem liệu nó thỏa mãn yêu cầu hay không. Ngược lại, trong tìm kiếm leo đồi, ta bắt đầu với một giải pháp khả dụng và cố gắng cải thiện nó thông qua các bước di chuyển nhỏ.

Một điểm quan trọng là tìm kiếm leo đồi sử dụng các giải pháp láng giềng của trạng thái hiện tại để kiểm tra và chuyển đến giải pháp tốt hơn. Điều này giúp giảm số lượng giải pháp phải tạo ra và kiểm tra, tăng cường hiệu suất của thuật toán.

Tuy tìm kiếm leo đồi không tạo ra nhiều giải pháp như thuật toán tạo và kiểm tra truyền thống, nhưng nó có thể là một lựa chọn hiệu quả hơn trong các bài toán tối ưu hóa nơi chúng ta không muốn kiểm tra mọi giải pháp có thể, mà chỉ quan tâm đến những giải pháp tiềm năng tốt hơn.

### 1.2.4 Sử dụng phương pháp Tham lam (Greedy)

Tại bất kỳ điểm nào trong không gian trạng thái, tìm kiếm chỉ di chuyển theo hướng tối ưu hóa hàm chi phí với hy vọng tìm ra giải pháp tối ưu nhất cuối cùng.

Khi sử dụng phương pháp Tham lam trong tìm kiếm leo đồi, thuật toán sẽ luôn chọn lựa giải pháp tốt nhất có sẵn tại mỗi bước di chuyển, mà không xem xét toàn bộ không gian trạng thái. Phương pháp này hữu ích trong việc giảm bớt độ phức tạp của thuật toán và tăng tốc quá trình tìm kiếm.

Cụ thể, tại mỗi trạng thái, thuật toán sẽ chọn trạng thái láng giềng nào có giá trị mục tiêu (hàm mục tiêu) tốt nhất và di chuyển đến đó. Điều này tương ứng với việc luôn lựa chọn con đường có vẻ tốt nhất ngay từ bước đầu tiên mà không quan tâm đến hậu quả của quá trình di chuyển.


## 1.3 Sơ đồ không gian trạng thái của thuật toán leo đồi

![[Hill_Climbing_State.png]]

Sơ đồ không gian trạng thái là một biểu diễn đồ họa không gian một chiều của tập hợp các trạng thái (đầu vào) mà thuật toán tìm kiếm của có thể đạt được so với giá trị của hàm mục tiêu tương ứng của chúng. Sơ đồ trạng thái có những điểm cần chú ý bao gồm:

- Cực đại toàn cục (Global maximum): Trạng thái tốt nhất có thể. Hàm mục tiêu trả về giá trị lớn nhất cho trạng thái này
- Cực đại cục bộ (Local maximum): Trạng thái tốt nhất so với các trạng thái liền kề với nó. Nhưng vẫn có trạng thái nào đó tốt hơn trạng thái này
- Vai đồi (Shoulder): Một đoạn đơn điệu ngang, nằm giữa sườn dốc
- Cực đại cục bộ phẳng (Flat local maximum): Một đoạn đơn điệu ngang nơi mà xung quang đoạn đơn điệu ngang đó không có các trạng thái tốt hơn
- Trạng thái hiện tại (Current state): Một điểm trên đồ thị biểu thị cho trạng thái hiện tại mà thuật toán đang xét trong không gian trạng thái

## 1.4 Ưu điểm của thuật toán leo đồi

Thuật toán leo đồi (hill climbing) có một số ưu điểm quan trọng, làm cho nó trở thành một phương pháp hữu ích trong nhiều bài toán tối ưu hóa:

1. **Đơn Giản và Dễ Hiểu:**
   - Thuật toán leo đồi là một thuật toán đơn giản và dễ hiểu, giúp nhanh chóng triển khai và thử nghiệm trong nhiều bối cảnh ứng dụng.

2. **Hiệu Quả Với Bài Toán Cục Bộ:**
   - Hiệu quả trong tìm kiếm cục bộ: Tìm kiếm trong lân cận của trạng thái hiện tại giúp giảm độ phức tạp và tăng tốc quá trình tìm kiếm.

3. **Tiết Kiệm Bộ Nhớ:**
   - Không yêu cầu lưu lại lịch sử tìm kiếm hoặc các trạng thái đã khảo sát, giảm yêu cầu bộ nhớ.

4. **Phù Hợp Cho Bài Toán Tối Ưu Hóa Đơn Biến:**
   - Đặc biệt hiệu quả cho các bài toán tối ưu hóa đơn biến, nơi giải pháp có thể được biểu diễn bằng một tập hợp giá trị thay đổi.

5. **Dễ Dàng Kết Hợp với Heuristic:**
   - Dễ dàng kết hợp với các phương pháp heuristic để cải thiện hiệu suất tìm kiếm.

6. **Tích Hợp Trong Hệ Thống Tìm Kiếm Lớn Hơn:**
   - Thường được tích hợp trong các hệ thống tìm kiếm lớn hơn, đặc biệt là trong các bước tìm kiếm cục bộ của các thuật toán tìm kiếm thông tin.

7. **Linh họat Trong Ứng Dụng Thực Tế:**
   - Có thể áp dụng trong nhiều lĩnh vực thực tế như lập lịch, tối ưu hóa tài nguyên, và quyết định trong môi trường có thay đổi.

## 1.5 Nhược điểm của thuật toán leo đồi

Thuật toán leo đồi (hill climbing) cũng có nhược điểm và hạn chế cần được xem xét khi áp dụng trong các bài toán cụ thể:

1. **Mắc Kẹt Ở Điểm Cực Đại Địa Phương:**
   - Một trong những nhược điểm lớn nhất của thuật toán leo đồi là khả năng mắc kẹt ở điểm cực đại địa phương, mà không thể vượt qua để đạt tới lời giải tối ưu toàn cục.

2. **Không Tìm Ra Lời Giải Toàn Cục:**
   - Không đảm bảo tìm ra lời giải tối ưu toàn cục, do thuật toán chỉ tập trung vào việc cải thiện từng bước di chuyển mà không thăm dò toàn bộ không gian trạng thái.

3. **Quá Phụ Thuộc Vào trạng thái bắt đầu:**
   - Kết quả cuối cùng có thể phụ thuộc lớn vào trạng thái bắt đầu. Nếu khởi đầu không tốt, thuật toán có thể không thể đạt đến lời giải tốt.

Tóm lại, điều quan trọng khi sử dụng thuật toán leo đồi là hiểu rõ tính chất của bài toán và chọn lựa các biến thể hoặc kết hợp với các phương pháp khác để giảm thiểu các nhược điểm trên.

# II. Các phiên bản của thuật toán leo đồi

## 2.1 Thuật toán leo đồi đơn giản (Simple hill Climbing)

### 2.1.1 Ý tưởng

Leo đồi đơn giản là cách đơn giản nhất để thực hiện thuật toán leo đồi. Nó chỉ đánh giá một trạng thái lân cận tại một thời điểm và chọn trạng thái đầu tiên tốt hơn trạng thái hiện tại và đặt nó làm trạng thái hiện tại.

### 2.1.2 Sơ đồ khối

![Simple](https://github.com/MinkuruDev/ObsidianBackup/raw/master/AI/Hill_Climbing_Simple.png)

***HÌnh**: Sơ đồ khối của thuật toán leo đồi đơn giản*

### 2.1.3 Mã giả

```ts
function simple_hill_climbing(start: State) -> State {
	let current = start;
	while true {
		let found_better = false;
		foreach neighbor in all_neighbor(current) {
			if heuristic(neighbor) > heuristic(current) {
				current = neighbor;
				found_better = true;
				break;
			}
		}
		
		if NOT found_better
			break;
	}

	return current;
}
```


## 2.2 Thuật toán leo đồi dốc đứng (Steepest-Ascent hill climbing)

### 2.2.1 Ý tưởng

Thuật toán này lựa chọn trong số tất cả hàng xóm hiện thời để tìm ra hàng xóm có hàm mục tiêu tốt nhất. Nếu hàng xóm đó tốt hơn trạng thái hiện thời thì di chuyển sang hàm xóm đó. Nếu không thì kết thúc và trả về trạng thái hiện thời

### 2.2.2 Sơ đồ khối

![Steepest](https://github.com/MinkuruDev/ObsidianBackup/raw/master/AI/Hill_Climbing_Steepest.png)

***HÌnh**: Sơ đồ khối của thuật toán leo đồi dốc đứng*

### 2.2.3 Mã giả

```ts
function steepest_hill_climbing(start: State) -> State {
	let current = start;
	while true {
		let best_neighbor = current;
		foreach neighbor in all_neighbor(current) {
			if heuristic(neighbor) > heuristic(best_neighbor) {
				best_neighbor = neighbor;
			}
		}
		
		if current == best_neighbor
			break;
	}

	return current;
}
```

# III. Các ứng dụng của thuật toán

Thuật toán leo đồi (hill climbing) thường được sử dụng trong nhiều ứng dụng khác nhau, đặc biệt là trong các bài toán tối ưu hóa cục bộ. Dưới đây là một số ứng dụng phổ biến của thuật toán leo đồi:

1. **Lập Lịch (Scheduling):**
   - Trong lập lịch công việc, thuật toán leo đồi có thể được áp dụng để tối ưu hóa thời gian hoặc tài nguyên cho mỗi công việc.

2. **Quy Hoạch Tài Nguyên (Resource Allocation):**
   - Trong quy hoạch tài nguyên, thuật toán này có thể được sử dụng để phân bổ tài nguyên (như máy chủ, băng thông) một cách hiệu quả.

3. **Tối Ưu Hóa Hệ Thống (System Optimization):**
   - Trong tối ưu hóa hệ thống, thuật toán leo đồi có thể được sử dụng để điều chỉnh các tham số hệ thống để cải thiện hiệu suất.

4. **Quyết Định Tác Động (Reinforcement Learning):**
   - Trong học tăng cường, thuật toán leo đồi có thể được sử dụng để tìm kiếm chính sách tốt nhất trong các tình huống tác động.

5. **Tìm Kiếm Trong Không Gian Tìm Kiếm (Search Spaces):**
   - Trong nhiều ứng dụng, thuật toán này được sử dụng để tìm kiếm trong không gian trạng thái hoặc tìm kiếm không gian giải pháp.

6. **Mô Hình Hóa và Dự Đoán (Modeling and Prediction):**
   - Trong việc mô hình hóa và dự đoán, thuật toán leo đồi có thể được sử dụng để điều chỉnh các tham số của mô hình để cải thiện dự đoán.

7. **Quyết Định Trong Trò Chơi (Game Playing):**
   - Trong trò chơi, thuật toán leo đồi có thể được sử dụng để đưa ra quyết định cho các tình huống cụ thể.

8. **Tối Ưu Hóa Đường Đi (Path Planning):**
   - Trong lập kế hoạch đường đi, thuật toán leo đồi có thể được sử dụng để tối ưu hóa đường đi từ điểm xuất phát đến điểm đích.

9. **Quyết Định Tối Ưu Trong Hệ Thống Điều Khiển (Optimal Control Systems):**
   - Trong điều khiển tối ưu, thuật toán này có thể được áp dụng để điều chỉnh các tham số để đạt được điều khiển tối ưu.

10. **Tìm Kiếm Tối Ưu Trong Mạng Lưới (Optimization in Networks):**
    - Trong mạng lưới, thuật toán leo đồi có thể được sử dụng để tối ưu hóa hiệu suất của các thành phần mạng.

Những ứng dụng trên chỉ là một số ví dụ và thuật toán leo đồi có thể được điều chỉnh và kết hợp với các phương pháp khác để phù hợp với nhiều bài toán và ngữ cảnh ứng dụng khác nhau.

# IV. Đánh giá thuật toán

## 4.1 Về thời gian thực hiện

Thời gian thực hiện của thuật toán tìm kiếm leo đồi đơn giản và leo đồi dốc đứng phụ thuộc vào nhiều yếu tố, bao gồm: 
- Kích thước của không gian tìm kiếm 
- Cách xác định các trạng thái lân cận 
- Cách tính toán hàm mục tiêu

Thuật Toán Leo Đồi Đơn Giản:
- Thời gian thực hiện của thuật toán leo đồi đơn giản phụ thuộc nhiều vào bài toán cụ thể và cách xác định các trạng thái lân cận. 
- Trong trường hợp xấu nhất, thời gian thực hiện có thể lớn, đặc biệt nếu không gian tìm kiếm lớn và không có sự tối ưu hoá trong việc chọn các trạng thái lân cận.

Thuật Toán Leo Đồi Dốc Đứng: 
- Thời gian cần thiết để leo đồi dốc đứng chọn được một hướng đi sẽ lớn hơn so với leo đồi đơn giản. 
- Tuy vậy, do lúc nào cũng chọn hướng đi tốt nhất nên leo đồi dốc đứng thường sẽ tìm đến lời giải sau một số bước ít hơn so với leo đồi đơn giản.

## 4.2 Về tính đầy đủ

Cả hai phương pháp leo núi đơn giản và leo núi dốc đứng đều có khả năng thất bại trong việc tìm lời giải của bài toán mặc dù lời giải đó thực sự hiện hữu. 
- Kết thúc khi đạt được một trạng thái mà không còn trạng thái nào tốt hơn nữa có thể phát sinh, nhưng trạng thái này không phải là trạng thái đích. 
- Điều này sẽ xảy ra nếu chương trình đạt đến một điểm cực đại địa phương, một đoạn đơn điệu ngang. 
**Do đó, cả hai thuật toán đều không đảm bảo tính đầy đủ.**

## 4.3 Về tính tối ưu

Thuật toán leo đồi đơn giản lựa chọn trạng thái hàng xóm tốt hơn đầu tiên mà nó gặp nên có thể dẫn đến vấn đề thực hiện nhiều chuyển động hơn để đạt được trạng thái đích, cho nên không đảm bảo tính tối ưu, tức là không chắc chắn tìm ra trạng thái đích với con đường tốt nhất.
Thuật toán leo đồi dốc đứng phải duyệt qua tất cả các hướng đi có thể có tại trạng thái hiện hành. Trong điều kiện đủ tốt, nếu thuật toán leo đồi dốc đứng có thể tìm được trạng thái đích thành công thì nó thường đảm tính tối ưu. Ở đây điều kiện cần xem xét như không gian trạng thái, vị trí xuất phát. Tuy nhiên, khi xem xét tổng quát cho mọi bài toán thì thuật toán này không đảm bảo tính tối ưu

## 4.4 Về độ phức tạp tính toán

Đánh giá độ phức tạp tính toán của hai thuật toán tìm kiếm leo đồi đơn giản và leo đồi dốc đứng đòi hỏi xem xét sâu hơn về : 

- cách tính toán hàm mục tiêu 
- cách xác định trạng thái lân cận 
- cách thuật toán thực hiện việc tìm kiếm

Thuật toán leo đồi dốc đứng tại mỗi bước phải xem xét tất cả các trạng thái lân cận để chọn ra trạng thái tốt nhất nên điều này tạo ra độ phức tạp tính toán lớn hơn so với thuật toán leo đồi đơn giản trong một bước.

## 4.5 Về yêu cầu bộ nhớ

Cả hai thuật toán leo đồi đơn giản và leo đồi dốc đứng không yêu cầu một lượng lớn bộ nhớ: 
- Chỉ cần lưu trữ trạng thái hiện tại và một số biến trung gian để thực hiện các phép toán cần thiết. 
- Không cần lưu trữ lịch sử của các trạng thái đã đi qua.
Do đó, về mặt yêu cầu bộ nhớ, cả hai thuật toán đều không đòi hỏi nhiều tài nguyên so với một số thuật toán tìm kiếm khác có độ phức tạp lớn hơn.

## 4.6 Tổng hợp

|Tiêu chí|Leo đồi đơn giản|Leo đồi dốc đứng|
|---|---|---|
|Tính đầy đủ|Không|Không|
|Tính tối ưu|Không|Không|
|Độ phức tạp tính toán|Nhỏ hơn|Lớn hơn|
|Yêu cầu bộ nhớ|Ít|Ít|


# V. Giới thiệu bài toán N quân hậu

Phát biểu của bài toán N quân hậu: Cho một bàn cờ vua có kích thước N * N và N quân hậu. Sắp xếp N quân hậu này lên bàn cờ vua sao cho không có cặp hậu nào có khả năng tấn công nhau. Một quân hậu có khả năng tấn công một quân cờ khác khi mà quân hậu nằm trên cùng một hàng ngang, một cột dọc hoặc một đường chéo với quân cờ đó

Bài toán N quân hậu được phát triển từ bài toán 8 quân hậu, lần đầu tiên xuất hiện vào thế kỷ XIX. Bài toán 8 quân hậu được Max Bezzel đưa ra vào năm 1848. 2 năm sau, vào năm 1850 Franz Nauck đưa ra lời giải cho bài toán 8 quân hậu và phát triển thành bài toán N quân hậu. Từ đó, các nhà toán học làm việc với cả 2 bài toán 8 quân hậu và bài toán tổng quát N quân hậu

Bài toán N quân hậu được chứng minh là có lời giải đối với mọi N >= 4

# VI. Áp dụng thuật toán leo đồi để giải quyết bài toán N quân hậu

## 6.1 Mô tả bài toán theo thuật toán leo đồi

Để ứng dụng thuật toán leo đồi giải quyết bài toán, bài toán N quân hậu có thể được mô tả như sau: 

- **Trạng thái**: Thông thường một bàn cờ cần có mảng 2 chiều để có thể lưu tất cả các trạng thái có thể của bàn cờ. Tuy nhiên cách dùng mảng 2 chiều sẽ khiến cho số trạng thái trở nên rất lớn `(N^2)!/((N^2-n)! * N!)`, vì vậy ta có thể dùng cách đơn giản hơn là dùng mảng 1 chiều để giảm đáng kể số lượng trạng thái. Mỗi trạng thái là sắp xếp của cả N quân hậu trên bàn cờ, sao cho mỗi hàng ngang chỉ gồm 1 quân. Mỗi trạng thái có thể được biểu diễn bằng một mảng 1 chiều với N phần tử. Phần tử thứ i của mảng thể hiện vị trí cột của quân hậu trên hàng thứ i. bằng cách sử dụng mảng 1 chiều thì tổng số trạng thái là N^N.
- **Trạng thái xuất phát**: khởi tạo ngẫu nhiên mảng 1 chiều N phần tử. Mỗi phần tử có giá trị trong khoảng từ 0 đến N-1
- **Hàm mục tiêu** (hàm heuristic) được tính bằng số các đôi hậu có khả năng tấn công lẫn nhau trong một trạng thái. (Nó được tính là tấn công khi hai quân cờ nằm trên cùng một đường thẳng/chéo, ngay cả khi có một quân cờ ở giữa chúng)
- **Mục tiêu**: Tìm cách đặt N quân hậu lên một bàn cờ N * N sao cho không có hai quân hậu nào cùng nằm trên cùng một hàng, cột hoặc đường chéo. Với hàm mục tiêu được tính ở trên, mục tiêu tìm kiếm là tối thiểu hóa giá trị của hàm mục tiêu (0 là giá trị tối ưu).
- **Chuyển động**: Chọn một hàng ngang, dịch chuyển quân hậu trong hàng ngang đó sang cột khác trên cùng hàng ngang đó.

## 6.2 Viết mã

Sử dụng cả 2 thuật toán leo đồi đơn giản và leo đồi dốc đứng để giải quyết bài toán N quân hậu theo cách nêu trên.

Đầu tiên, ta viết các đoạn mã dùng chung cho cả 2 thuật toán để thực hiện các chức năng như: tính số lượng cặp hậu đối nhau, hàm in ra bàn cờ, hàm khởi tạo trạng thái ngẫu nhiên. Các hàm này được viết trong file `utils.h`:

```cpp
// utils.h
#include <bits/stdc++.h>

using namespace std;

// hàm đánh giá
// trả về số lượng cặp hậu có thể tấn công nhau
int computeThreats(const vector<int>& state) {
    int threats = 0;
    int size = state.size();
    for (int i = 0; i < size; i++) {
        for (int j = i + 1; j < size; j++) {
            if (state[i] == state[j] || abs(state[i] - state[j]) == j - i) {
                threats++;
            }
        }
    }
    return threats;
}
  
// hàm vẽ bàn cờ, in ra bàn cờ gồm 0 và 1
// biều thị cho ô trống và ô có quân hậu
void printBoard(const vector<int>& state) {
    int size = state.size();
    for (int row = 0; row < size; row++) {
        for (int col = 0; col < size; col++) {
            if (col == state[row]) cout << "1 ";
            else cout << "0 ";
        }
        cout << endl;
    }
}
  
// hàm tạo trạng thái ngẫu nhiên
vector<int> randomState(int n){
    // khởi tạo seed random
    srand(time(NULL));
    
    vector<int> result(n);
  
    for(int i = 0; i<n; i++){
        result[i] = rand() % n;
    }
  
    return result;
}
```


Mã C++ để giải quyết bài toán N quân hậu bằng thuật toán leo dồi đơn giản

```cpp
#include "utils.h"
  
vector<int> simpleHillClimbing(vector<int> state) {
    int step = 0;
    int size = state.size();
  
    while (true) {
        int currentThreats = computeThreats(state);
        bool foundBetter = false;
  
        // Duyệt các trạng thái kề với trạng thái hiện tại
        // (N*N - N) trạng thái
        for (int row = 0; row < size; row++) {
            for (int col = 0; col < size; col++) {
                if (col == state[row])
                    continue;
  
                vector<int> newState = state;
                // đặt trạng thái mới
                newState[row] = col;
                int newThreats = computeThreats(newState);
                if (newThreats < currentThreats) {
                    // gán trạng thái hiện tại là trạng thái mới
                    state = newState;
                    currentThreats = newThreats;
                    foundBetter = true;
                    cout << "Thuat toan leo doi don gian: " << endl;
                    cout << "So lan thay doi trang thai: " << ++step << endl;
                    printBoard(state);
                    cout << "So cap hau doi nhau: " << currentThreats << endl << endl;
                    break;  // Thoát vòng lặp trong
                }
            }
            if (foundBetter) break;  // Thoát vòng lặp ngoài
        }
        // Khi không còn trạng thái tốt hơn
        if (!foundBetter) {
            return state;
        }
    }
}
  
int main() {
	int n;
    cout << "Nhap N: "; cin >> n;
    vector<int> initial = randomState(n);
  
    cout << "Trang thai bat dau: " << endl;
    printBoard(initial);
    cout << "So cap hau doi nhau: " << computeThreats(initial) << endl << endl;
  
    vector<int> resultSimple = simpleHillClimbing(initial);
  
    cout << "\nTrang thai ket thuc thuat toan leo doi don gian: " << endl;
    printBoard(resultSimple);
    cout << "So cap hau doi nhau: " << computeThreats(resultSimple) << endl << endl;
  
    return 0;
}
```

Mã C++ để giải quyết bài toán N quân hậu bằng thuật toán leo dồi dốc đứng

```cpp
#include "utils.h"

vector<int> steepestAscentHillClimbing(vector<int> state) {
    int step = 0;
    int size = state.size();
  
    while (true) {
        int currentThreats = computeThreats(state);
        vector<int> bestState = state;
        int bestThreats = currentThreats;
  
        // Duyệt các trạng thái kề với trạng thái hiện tại
        for (int row = 0; row < size; row++) {
            for (int col = 0; col < size; col++) {
                if (col == state[row])
                    continue;
  
                vector<int> newState = state;
                newState[row] = col;
                int newThreats = computeThreats(newState);
                if (newThreats < bestThreats) {
                    bestState = newState;
                    bestThreats = newThreats;
                }
            }
        }
  
        // Không còn trạng thái kề tốt hơn
        // trạng thái hiện tại
        if (bestThreats == currentThreats)
            return state;
  
        state = bestState;
        cout << "Thuat toan leo doi doc dung: " << endl;
        cout << "So lan thay doi trang thai: " << ++step << endl;
        printBoard(state);
        cout << "So cap hau doi nhau: " << bestThreats << endl << endl;
    }
}
  
int main() {
    int n;
    cout << "Nhap N: "; cin >> n;
    vector<int> initial = randomState(n);
    
    cout << "Trang thai bat dau: " << endl;
    printBoard(initial);
    cout << "So cap hau doi nhau: " << computeThreats(initial) << endl << endl;
  
    vector<int> resultSteepest = steepestAscentHillClimbing(initial);
  
    cout << "\nTrang thai ket thuc thuat toan leo doi doc dung: " << endl;
    printBoard(resultSteepest);
    cout << "So cap hau doi nhau: " << computeThreats(resultSteepest) << endl;
  
    return 0;
}
```

# VII. Demo

Kết quả chạy chương trình để giải quyết bài toán N quân hậu với thuật toán leo đồi đơn giản:

```
Nhap N: 8
Trang thai bat dau: 
0 0 0 0 0 1 0 0
0 0 1 0 0 0 0 0
1 0 0 0 0 0 0 0
0 0 0 1 0 0 0 0
0 0 0 0 0 0 1 0
0 0 0 1 0 0 0 0
0 0 0 0 0 1 0 0
0 0 0 0 0 0 1 0
So cap hau doi nhau: 5

Thuat toan leo doi don gian:
So lan thay doi trang thai: 1
0 0 0 0 1 0 0 0
0 0 1 0 0 0 0 0
1 0 0 0 0 0 0 0
0 0 0 1 0 0 0 0
0 0 0 0 0 0 1 0
0 0 0 1 0 0 0 0
0 0 0 0 0 1 0 0
0 0 0 0 0 0 1 0
So cap hau doi nhau: 4

Thuat toan leo doi don gian:
So lan thay doi trang thai: 2
0 0 0 0 1 0 0 0
0 0 1 0 0 0 0 0
0 0 0 0 0 0 0 1
0 0 0 1 0 0 0 0
0 0 0 0 0 0 1 0
0 0 0 1 0 0 0 0
0 0 0 0 0 1 0 0
0 0 0 0 0 0 1 0
So cap hau doi nhau: 3

Thuat toan leo doi don gian:
So lan thay doi trang thai: 3
0 0 0 0 1 0 0 0
0 0 1 0 0 0 0 0
0 0 0 0 0 0 0 1
0 0 0 1 0 0 0 0
0 1 0 0 0 0 0 0
0 0 0 1 0 0 0 0
0 0 0 0 0 1 0 0
0 0 0 0 0 0 1 0
So cap hau doi nhau: 2

Thuat toan leo doi don gian:
So lan thay doi trang thai: 4
0 0 0 0 1 0 0 0
0 0 1 0 0 0 0 0
0 0 0 0 0 0 0 1
0 0 0 1 0 0 0 0
0 1 0 0 0 0 0 0
0 0 0 1 0 0 0 0
0 0 0 0 0 1 0 0
1 0 0 0 0 0 0 0
So cap hau doi nhau: 1


Trang thai ket thuc thuat toan leo doi don gian:
0 0 0 0 1 0 0 0
0 0 1 0 0 0 0 0
0 0 0 0 0 0 0 1
0 0 0 1 0 0 0 0
0 1 0 0 0 0 0 0
0 0 0 1 0 0 0 0
0 0 0 0 0 1 0 0
1 0 0 0 0 0 0 0
So cap hau doi nhau: 1
```

Kết quả chạy thuật toán leo đồi đơn giản nhiều lần với N=8:

|Lần|Trạng thái bắt đầu|Số cặp hậu đối nhau khi bắt đầu|Trạng thái kết thúc|Số cặp hậu đối nhau khi kết thúc|Số lần thay đổi trạng thái|
|---|---|---|---|---|---|
|1|[5,2,0,3,6,3,5,6]|5|[4,2,7,3,1,4,6,0]|1|4|
|2|[1,7,1,0,6,6,3,1]|8|[4,7,5,2,6,6,3,1]|2|5|
|3|[2,4,7,0,6,0,0,4]|7|[3,5,7,1,6,0,2,4]|0|6|
|4|[7,2,0,6,0,4,5,3]|4|[4,2,0,6,1,7,5,3]|0|3|
|5|[2,2,7,7,3,5,3,7]|8|[6,2,0,1,3,5,3,7]|3|3|
|6|[4,7,2,3,0,4,1,6]|6|[5,7,0,3,0,4,1,6]|2|3|
|7|[0,4,2,4,5,7,2,1]|5|[6,0,2,4,7,7,3,1]|1|4|
|8|[6,3,1,2,0,3,1,7]|4|[6,4,7,2,0,3,1,7]|2|2|
|9|[7,2,1,1,1,7,6,1]|9|[3,5,0,4,1,7,6,2]|1|6|
|10|[7,3,5,1,7,5,6,4]|7|[1,3,0,2,7,5,6,4]|2|4|


Kết quả chạy chương trình để giải quyết bài toán N quân hậu với thuật toán leo đồi dốc đứng:

```
Nhap N: 8
Trang thai bat dau: 
0 1 0 0 0 0 0 0
0 1 0 0 0 0 0 0
0 0 0 0 0 0 1 0
0 0 0 0 1 0 0 0
1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 1
0 0 0 0 0 0 0 1
0 0 1 0 0 0 0 0
So cap hau doi nhau: 5

Thuat toan leo doi doc dung:
So lan thay doi trang thai: 1
0 0 0 1 0 0 0 0
0 1 0 0 0 0 0 0
0 0 0 0 0 0 1 0
0 0 0 0 1 0 0 0
1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 1
0 0 0 0 0 0 0 1
0 0 1 0 0 0 0 0
So cap hau doi nhau: 2

Thuat toan leo doi doc dung:
So lan thay doi trang thai: 2
0 0 0 1 0 0 0 0
0 1 0 0 0 0 0 0
0 0 0 0 0 0 1 0
0 0 0 0 1 0 0 0
1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 1
0 0 0 0 0 1 0 0
0 0 1 0 0 0 0 0
So cap hau doi nhau: 0


Trang thai ket thuc thuat toan leo doi doc dung:
0 0 0 1 0 0 0 0
0 1 0 0 0 0 0 0 
0 0 0 0 0 0 1 0
0 0 0 0 1 0 0 0
1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 1
0 0 0 0 0 1 0 0
0 0 1 0 0 0 0 0
So cap hau doi nhau: 0
```

Kết quả chạy thuật toán leo đồi dốc đứng nhiều lần với N=8:

|Lần|Trạng thái bắt đầu|Số cặp hậu đối nhau khi bắt đầu|Trạng thái kết thúc|Số cặp hậu đối nhau khi kết thúc|Số lần thay đổi trạng thái|
|---|---|---|---|---|---|
|1|[1,1,6,4,0,7,7,2]|5|[3,1,6,4,0,7,5,2]|0|2|
|2|[6,4,2,2,4,3,0,7]|8|[2,4,1,7,5,3,0,7]|1|4|
|3|[5,2,1,1,1,2,6,3]|7|[5,0,4,1,7,2,6,3]|0|3|
|4|[2,2,3,2,7,0,0,5]|8|[2,6,3,1,7,4,0,5]|1|3|
|5|[5,3,5,7,6,2,4,2]|7|[5,3,1,7,0,6,4,2]|2|3|
|6|[1,7,2,5,1,2,1,1]|12|[1,6,2,5,7,4,0,3]|0|5|
|7|[5,5,5,7,6,2,5,3]|11|[5,7,0,4,6,2,5,3]|3|4|
|8|[4,5,3,0,5,0,6,7]|6|[2,7,1,1,5,0,6,3]|1|5|
|9|[4,5,3,6,3,2,4,3]|7|[0,5,3,6,0,2,4,1]|1|3|
|10|[7,5,2,3,2,7,6,0]|10|[1,4,1,5,2,7,3,0]|2|5|

# VIII. Giải pháp tối ưu hơn để giải quyết bài toán N quân hậu

Thuật toán leo đồi không đòi hỏi nhiều bộ nhớ để chạy và cũng không duyệt hết tất cả các trạng thái. Tuy nhiên việc quá phụ thuộc vào trạng thái bắt đầu khiến cho việc muốn giải bài toán thì phải chạy nhiều lần với các trạng thái ban đầu khác nhau mà chưa chắc tìm được lời giải. Tại mỗi lần xét các trạng thái lân cận, thuật toán leo đồi dốc đứng xét N * (N-1) trạng thái láng giềng, mỗi trạng thái láng giềng cần kiểm tra số cặp hậu đối nhau trong N * N bước. Độ phức tạp thời gian cho một lần xét tất cả các trạng thái láng giềng lên đến O(N^4), khiến cho thuật toán tốn rất nhiều thời gian với N lớn.

Để giải quyết bài toán N quân hậu một cách hiệu quả trong O(N), khi mà bài toán chỉ yêu cầu đưa ra một lời giải bất kỳ. Bài toán có công thức chung để đưa ra một lời giải cho tất cả N >= 4. Công thức đó có thể được trình bày như sau:

1. Liệt kê 2 danh sách. một danh sách số chẵn từ 2 đến N và một danh sách số lẻ từ 1 đến N
2. Chia N cho 6, lấy số dư r. Nếu r không phải 2 hoặc 3 thì giữ nguyên danh sách đó
3. Nếu r = 2, đổi chỗ của 1 và 3 trong danh sách số lẻ. Đẩy 5 xuống cuối cùng trong danh sách số lẻ
4. Nếu r = 3, đẩy 2 xuống cuối trong danh sách số chẵn. Đẩy 1 và 3 xuống cuối cùng của danh sách số lẻ
5. Sát nhập danh sách số lẻ vào cuối của danh sách số chẵn. Danh sách nhận được là một kết của của bài toán N quân hậu bằng cách đặt quân hậu của cột số `i` lên hàng ngang `list[i]`

Ví dụ về lời giải:

- N = 14, r = 2: 2, 4, 6, 8, 10, 12, 14, 3, 1, 7, 9, 11, 13, 5.
- N = 15, r = 3: 4, 6, 8, 10, 12, 14, 2, 5, 7, 9, 11, 13, 15, 1, 3.
- N = 20, r = 2: 2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 3, 1, 7, 9, 11, 13, 15, 17, 19, 5.

Bằng cách sử dụng công thức đã được chứng minh, ta có thể giải quyết bài toán N quân hậu với độ phức tạp về thời gian và bộ nhớ là O(N)

Công thức trên sử dụng bàn cờ N * N được đánh số từ 1 đến N. Để tái sử dụng mã c++ của hàm `printBoard` và `computeThreads` (sử dụng chỉ số từ 0 đến N-1), ta biến đổi thuật toán trên một chút để phù hợp với viết mã mà vẫn giữ nguyên bản chất của công thức:

1. Liệt kê 2 danh sách. một danh sách số chẵn từ 0 đến N-1 và một danh sách số lẻ từ 1 đến N-1
2. Chia N cho 6, lấy số dư r. Nếu r không phải 2 hoặc 3 thì giữ nguyên danh sách đó
3. Nếu r = 2, đổi chỗ của 0 và 2 trong danh sách số chẵn. Đẩy 4 xuống cuối cùng trong danh sách số chẵn
4. Nếu r = 3, đẩy 1 xuống cuối trong danh sách số lẻ. Đẩy 0 và 2 xuống cuối cùng của danh sách số chẵn
5. Sát nhập danh sách số chẵn vào cuối của danh sách số lẻ. Danh sách nhận được là một kết của của bài toán N quân hậu bằng cách đặt quân hậu của hàng ngang số `i` lên cột `list[i]`

Mã c++ để giải quyết bài toán N quân hậu theo công thức

```cpp
#include "utils.h"

// giải bài toán N quân hậu bằng công thức
vector<int> nQueensSolve(int n){
    // tạo 2 danh sách chẵn và lẻ
    vector<int> odd, even;
  
    if(n % 6 == 2){ // trường hợp r = 2
        // đổi chỗ 0 và 2 trong danh sách số chẵn
        even.push_back(2);
        even.push_back(0);
        for(int i = 6; i<n; i+=2)
            even.push_back(i);
        // đẩy 4 xuống cuối trong danh sách số chẵn
        even.push_back(4);
  
        for(int i = 1; i<n; i+=2)
            odd.push_back(i);
  
    }else if(n % 6 == 3){ // trường hợp r = 3
        for(int i = 3; i<n; i+=2)
            odd.push_back(i);
        // đẩy 1 xuống cuối trong danh sách số lẻ
        odd.push_back(1);
  
        for(int i = 4; i<n; i+=2)
            even.push_back(i);
        // đẩy 0 và 2 xuống cuối trong danh sách số chẵn
        even.push_back(0);
        even.push_back(2);
  
    }else{
        for(int i = 1; i<n; i+=2)
            odd.push_back(i);
        for(int i = 0; i<n; i+=2)
            even.push_back(i);
    }

	// gộp 2 list lại trả ra kết quả
    odd.insert(odd.end(), even.begin(), even.end());
    return odd;
}
  
int main(){
	int n;
    cout << "Nhap N: "; cin >> n;
    vector<int> result = nQueensSolve(n);
    printBoard(result);
    cout << "So cap hau doi nhau: " << computeThreats(result) << endl;
}
```

Kết quả chạy tìm lời giải cho bài toán N quân hậu theo công thức:

```
Nhap N: 20
0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 
0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1
0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0
0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
So cap hau doi nhau: 0
```

# Kết luận

# Tài liệu tham khảo

1. Từ Minh Phương, (2014) **Giáo trình Nhập môn trí tuệ nhân tạo**, Học viện Công nghệ bưu chính viễn thông. 
2. Russell, S.J., Norvig, P. and Chang, M. (2022) **Artificial intelligence a modern approach**. Harlow: Pearson Education Limited. 
3. Bo Bernhardsson, **Explicit Solutions to the N-Queens Problem for all N** (no date)
4. 
5. **AI: Search algorithms: Hill Climbing** (no date) Codecademy. Available at: https://www.codecademy.com/resources/docs/ai/search-algorithms/hill-climbing 
6. **Introduction to hill climbing: Artificial intelligence** (2023) GeeksforGeeks. Available at: https://www.geeksforgeeks.org/introduction-hill-climbing-artificial-intelligence/ 
7. **Trí Tuệ Nhân Tạo: Cấu trúc chung của bài toán tìm kiếm**, (no date) VOER. Available at: https://voer.edu.vn/c/cau-truc-chung-cua-bai-toan-tim-kiem/b371101c/9795103b
8. Wikipedia: **Eight queens puzzle** https://en.wikipedia.org/wiki/Eight_queens_puzzle