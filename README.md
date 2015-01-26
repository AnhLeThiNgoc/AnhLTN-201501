# AnhLTN-201501
create a game about snail for html5 and canvas

link tham khao: http://www.ibm.com/developerworks/library/j-html5-game5/
Chương 5: Thưc hiện các hành vi của Sprite

Các nhân vật của Snail Bait được trang bị nhiều hành vi.
các hành vi :
- runner chạy
- nút tốc độ qua lại trên bậc thềm của chúng
- hồng ngọc và ngọc bích lấp lánh
Bảng các hành vi tương ứng với các nhân vật

Buttons, snails: paceBehavior -> nút qua lại lại trên bệc thềm
Runner : runBehavior -> Chu kỳ qua hình ảnh của runner để làm nó như đang chạy
Snail : snailShootBehavior -> bắn 1 quả bom từ miệng ốc sên
Snail: cycleBehavior -> Chu kỳ qua ảnh của Sprite
Snail bomd : snailBombMoveBehavior -> Di chuyển quả bom của ốc theo chiều ngang sang bên trái trong khung hình có thể nhìn thấy của bức tranh

Trong chương này chúng ta sẽ thảo luận các hành vi sau:
- Thực hiện các hành vi và gán cho sprites
- Chu kỳ 1 sprite qua 1 chuỗi các ảnh
-  Tạo ra hánh vi bay và lưu trên bộ nhớ người sử dụng
- Kết hợp các hành vi
- Sử dụng các hành vi để bắn đạn
Nguyên tắc cơ bản của hành vi
	Bất kỳ đối tượng nào có hành vi nó có method execute(). Phương thức này có 3 đối: 1 sprite, thời gian, tỷ lệ khung hình cho hiệu ứng của hình ảnh. Một hành vi của phương thức execute() làm thay đổi trạng thái của sprite sự trên thời gian và tỷ lệ khung hình hiệu ứng (hình ảnh động).
	các hành vi này là cần thiết bởi vì:
		- Họ tách rời sprite từ cách ứng xử của chúng
		- Bạn có thể thay đổi hành vi sprite trong thời gian chúng chạy
		- Bạn co thể thực hiện hành vi với bất kỳ sprite nào
		- 
Các hành vi của runner
Runner của Snail Bait có 4 hành vi sau:
runBehavior : Chu kỳ thông qua các cell của runner từ sprite sheet để tạo người đang chạy 
jumpBehavior: Kiểm soát mọi chuyển động của việc nhảy: đi lên, sự xuống, hạ cánh
fallBehavior: kiểm soát mọi chuyển động theo chiều dọc giống như runner rơi
runnerCollideBehavior: phát hiện và phản ứng lại, va chạm với các sprite khác
Listing 1: là 1 mảng các hành vi của runner qua hàm dựng Sprite.
Listing 2: các thuộc tính hành vi của runner
Listing 3: với mỗi khung hình ảnh động, cần gọi method upadte()
	phương thức Sprite.update() lặp đi lặp lại trên các hành vi của sprite, mỗi khi gọi phương thức execute(). Liên tục gọi tất cả các hình vi của các sprite cho mỗi khung hình động. mỗi một hành vi của phương thức execute(), không giống với các phương thức khác, các mà được gọi tương đối thường xuyên; thay vào đó mỗi phương thức execute() giống như một động cơ nhỏ cái mà liên tục chạy.
 	Bây giờ bạn hiểu các sprite và các hành vi của chúng kết hợp với nhau thế nào, tôi sẽ tập trung thực hiện chúng một cách riêng rẽ.
Running
	Snail Bait có 2 điều làm cho nó xuất hiện như runner đang chạy. đầu tiên, như tối đã thảo luận trong phấn "Scrolling the background" trong bài viết thứ 2 article là 1 chuỗi, game liên tục được cuộn theo background, làm nó xuất hiện runner di chuyển theo chiều ngang. Thứ 2, chu kỳ hành vi chạy của runner qua 1 chuỗi các ảnh của sprite sheet.
Listig 4: thực hiện hành vi chạy của runner = runBehavior
	phương thức execute() của runBehavior sự next các image.
	runBehavior tiến triển việc xác định  ảnh runner chạy nhanh như thế nào. đó là thời gian cần thêm thuộc tính runAnimationRate. Runner ở trạng thái không chạy khi game start, vì vậy runAnimationRate khởi tạo bằng 0. Khi người chơi lần lượt từ trái hoặc phải, tuy nhiên, Snail Bait cái đặt thuộc tính 17frames/second, như trong Listing 5, và runner bắt đầu chạy.
	phương thức turnLeft() và turnRight(), cái mà được xử lý bằng sự kiện bàn phím, kiểm soát một cách nhanh chóng chu kỳ của runner qua chuỗi ảnh với thuộc tính runAnimationRate, như tôi đã tháo luận trước đó. phương thức đó cũng điều khiển sự di chuyển của runner tốc độ từ trái sang phải thông qua thuộc tính bgVelocity, cái mà đại diện cho tỷ lệ khung nề cuộn.
Flyweight behaviors (hành vi trọng lượng bay)
	Hành vi chạy của runner được thảo luận trong phần duy trì trạng thái ở trước, cụ thể là hành vi cuối cùng tiến tới ảnh của sprite. Trạng thái của cặp runner làm nên hành vi. vì vậy, ví dụ nếu bạn muốn sprite khác chạy, bạn cần có hành vi run.
	Hành vi không duy tri trạng thái là linh hoạt hơn, ví dụ họ có thể sử dụng như flyweights. một flyweight là trường hợp duy nhất của đối tượng, được sử dụng bởi một vài đối tượng cùng một lúc. Hình 3 mô tả diều đó. một trường hơp duy nhấ của hành vi là sử dụng game's button và snail của nó, tất cả đều có tốc độ qua lại trên bậc thềm như hình 3.
Listing 6: createButtonSprites() thêm hành vi nhịp điệu (bước) duy nhất của mỗi button
Listing 7: paceBehavior: hành vi bước (nhịp điệu, tốc độ chạy)
	Hành vi tốc đọ chạy thay đổi 1 vị trí nằm ngang của sprite. hành vi chuyển động theo thời gian để tính toán bao nhiêu điểm ảnh để di chuyển cho khung hình hiện tại bằng cách chia vận tốc của sprite với tỷ lệ khung hình ảnh động, cái mà có kết quả là pixels/frame.
hành vi của trò chơi không rõ ràng 
	Hành vi đầu tiên tôi đã thảo luận trong bài thảo luận này - runBehavior - là một hành vi chặt chẽ với 1 sprite duy nhất. paceBehavior, cái mà thảo luận tiếp theo, là một hành vi không chính thống cái mà tách riêng từ cá nhân sprite do đó một trường hợp duy nhất có thể sử dụng nhiều sprite.
	Các hành vi có thể tổng quá hơn nữa: ban có thể tách chúng từ sprite các nhân (bản thân nó), nhưng cũng có thẻ từ chính trò chơi của nó. Snail Bait sử dụng ba hành vi có thể sử dụng trong bất kỳ trò chơi nào:
		- bounceBehavior -  sự nhảy lên
		- cycleBehavior - vòng đợi, chu kỳ
   		- pulseBehavior - nhịp điệu
	Hành vi nhảy lên và xuống, chu kỳ của sprite được thiết lập qua các image, và hành vi nhịp điệu điều khiển độ mở của sprite làm cho nó xuất hiện như thể dao động.
	Hành vi nhảy lên và nhịp điệu liên quan đến hình ảnh động phi tuyến, cái mà tôi sẽ thảo luận trong bài viết tiếp theo. Hành vi vong đời thể hiện qua các image, tuy nhiên, vì vậy tôi sẽ sử dụng thực hiện hành vi đó để minh hoạ cho hành vi thực hiện trong bất kỳ trò chơi nào.

Hồng ngọc lấp lánh (sparkling rubies)
	Snail Bait 's rubies and sapphires (ngọc bích) sparkle như hình 4 dưới đây.
	
	sprite sheet của snail bait là một chuỗi các ảnh của cả hồng ngọc và ngọc bích; chu kỳ của các cảnh tạo ra các ảo ảnh lấp lánh.
	Listing 8 là phương thức tạo các hồng ngọc. Tương tụ với phương thức tạp các ngọc bích. createRubySprites() cũng tạo ra vòng đời mỗi 500ms hiển thị ảnh tiếp theo từ 1 chuỗi hồng ngọc lấp lánh từ 100ms.
	Listing 8: Creating rubies
	Líting 9: CycleBehavior() - 
		chu kỳ các hành vi sẽ làm việc với bất kỳ sprite nào có a sprite sheet artist, điều đó có nghĩa là hành vi là không cụ thể cho Snail Bait và có thể tái sử dụng trong các game khác.
Sự kết hợp các hành vi
	các hành vi của từng cá nhân như chạy, đi đi lại lại, lấp lánh. Bạn có thể kết hợp các hành vi đó lại cho các hiệu ứng phức tạp hơn ví dụ như những con ốc sên bò trên bậc thềm bẵn những quả bom như trong Hình 5
	Một chuỗi các hành vi bắn súng của ốc sên:
		- paceBehavior: hành vi bước đi
		- snailShootBehavior:  bắn súng của ốc sên
		- snailBombMoveBehavior: di chuyển bom của ốc sên
	paceBehavior và snailShootBehavior được kết hơp với ốc sên; snailBombMoveBehavior được kết hợp với bom của ốc sên, khi Snail Bait tạo sprites, nó chỉ rõ 2 hành vi đầu tiên trong hàm dựng Sprite, như bạn có thể thấy ở Listing 10
	Listing 10: cretating snails
	với mỗi 1.5 s, vòng đời CyleBehavior() qua các ảnh của sanil trong sprite sheet, như hình 6 dưới đây, và mỗi ảnh hiển thị 300ms, cái mà làm nó trông giống như ốc sên mở và đóng miệng. paceBehavior() của ốc sên thể hiện sự di chuyển của ốc sên trên bậc thềm.
	Bom ốc sên được tạo ra bởi armSnails(), như Listing 11, cái mà Snail Bait gọi khi bắt đầu game. Đó là phương pháp lặp trên game snail, tạo bom ốc sên với mỗi 1 ốc sên, trang bị mỗi quả bom 1 snailBombMoveBehavior(), và lưu 1 tham chiếu tới ốc sên trong bom ốc sên.
	Listing 11: Ảming snails: Tạo bom ốc sên
	Listing 12: Shooting snail bombs: bắn bom của ốc sên - snailShootBehavior()
	Bởi vì snailShootBehavior được kết hợp với ốc sên, sprite đư vào phương thức execute() là ốc sên.
	Một ốc sên duy trì một tham chiếu tới bom ốc sên của nó, vì vậy snailShootBehavior điều khiển bom thông quâ ốc sên. snailShootBehavior sau đó kiểm tra nếu hình ảnh hiện tại của ốc là một trong những hình ảnh bên phải trong hình 6, có nghĩa là ốc sên trên bờ vưc của mở miêngj của nó. trong trường hợp đó, hành vi đặt bom tromg miệng ốc sên và làm cho nó có thể nhìn thấy được.
	bom ốc sên đang bắn, do đó, bao gồm vị trí của bom và làm nó làm nó có thể nhìn 
	Listing 8: Creating rubies
	Líting 9: CycleBehavior() - 
		chu kỳ các hành vi sẽ làm việc với bất kỳ sprite nào có a sprite sheet artist, điều đó có nghĩa là hành vi là không cụ thể cho Snail Bait và có thể tái sử dụng trong các game khác.
Sự kết hợp các hành vi
	các hành vi của từng cá nhân như chạy, đi đi lại lại, lấp lánh. Bạn có thể kết hợp các hành vi đó lại cho các hiệu ứng phức tạp hơn ví dụ như những con ốc sên bò trên bậc thềm bẵn những quả bom như trong Hình 5
	Một chuỗi các hành vi bắn súng của ốc sên:
		- paceBehavior: hành vi bước đi
		- snailShootBehavior:  bắn súng của ốc sên
		- snailBombMoveBehavior: di chuyển bom của ốc sên
	paceBehavior và snailShootBehavior được kết hơp với ốc sên; snailBombMoveBehavior được kết hợp với bom của ốc sên, khi Snail Bait tạo sprites, nó chỉ rõ 2 hành vi đầu tiên trong hàm dựng Sprite, như bạn có thể thấy ở Listing 10
	Listing 10: cretating snails
	với mỗi 1.5 s, vòng đời CyleBehavior() qua các ảnh của sanil trong sprite sheet, như hình 6 dưới đây, và mỗi ảnh hiển thị 300ms, cái mà làm nó trông giống như ốc sên mở và đóng miệng. paceBehavior() của ốc sên thể hiện sự di chuyển của ốc sên trên bậc thềm.
	Bom ốc sên được tạo ra bởi armSnails(), như Listing 11, cái mà Snail Bait gọi khi bắt đầu game. Đó là phương pháp lặp trên game snail, tạo bom ốc sên với mỗi 1 ốc sên, trang bị mỗi quả bom 1 snailBombMoveBehavior(), và lưu 1 tham chiếu tới ốc sên trong bom ốc sên.
	Listing 11: Ảming snails: Tạo bom ốc sên
	Listing 12: Shooting snail bombs: bắn bom của ốc sên - snailShootBehavior()
	Bởi vì snailShootBehavior được kết hợp với ốc sên, sprite đư vào phương thức execute() là ốc sên.
	Một ốc sên duy trì một tham chiếu tới bom ốc sên của nó, vì vậy snailShootBehavior điều khiển bom thông quâ ốc sên. snailShootBehavior sau đó kiểm tra nếu hình ảnh hiện tại của ốc là một trong những hình ảnh bên phải trong hình 6, có nghĩa là ốc sên trên bờ vưc của mở miêngj của nó. trong trường hợp đó, hành vi đặt bom tromg miệng ốc sên và làm cho nó có thể nhìn thấy được.
	bom ốc sên đang bắn, do đó, bao gồm vị trí của bom và làm nó làm nó có thể nhìn thấy trong điều kiện đúng. Sau đó việc di chuyển bom là nhiệm vụ của snailBombMoveBehavior,  như trong Listing 13
	Listing 13: Snail bomb move behavior
	Chừng nào bom ốc hiển thị trong tầm nhìn, snailBombMoveBehavior di chuyển bom sang bên trái với tốc đọ của sanilBait.SNAIL_BOMB_VELOCITY(450)pixels/second. Khi bom không trong tầm nhìn thì làm cho nó vô hình.

	


