# Knight-s-Tour
Dùng d3.js vẽ hành trình của 1 quân Mã trên bàn cờ kích thước nxn
## Nguồn gốc bài toán
* Mã đi tuần (hay hành trình của quân mã) là bài toán về việc di chuyển một quân mã trên bàn cờ vua (8 x 8). 
Quân mã được đặt ở một ô trên một bàn cờ trống nó phải di chuyển theo quy tắc của cờ vua để đi qua mỗi ô trên bàn cờ đúng một lần.
Có rất nhiều lời giải cho bài toán này, chính xác là 26.534.728.821.064 lời giải trong đó quân mã có thể kết thúc tại chính ô mà nó khởi đầu.
Một hành trình như vậy được gọi là hành trình đóng. Có những hành trình, trong đó quân mã sau khi đi hết tất cả 64 ô của bàn cờ (kể cả ô xuất phát), thì từ ô cuối của hành trình không thể đi về ô xuất phát chỉ bằng một nước đi. Những hành trình như vậy được gọi là hành trình mở.
* Một hành trình của quân mã với bàn cờ 8x8 như hình vẽ dưới đây:

![Knight's Tour](Knight's_tour_anim.gif)

## Phân tích bài toán
* Cho bàn cờ có kích thước n x n (có nxn ô). Một quân mã được đặt tại ô ban đầu có toạ độ x 0 , y 0
và được phép dịch chuyển theo luật cờ thông thường. Bài toán đặt ra là từ ô ban đầu, tìm một chuỗi
các nước đi của quân mã, sao cho quân mã này đi qua tất cả các ô của bàn cờ, mỗi ô đúng 1 lần.
* Như đã nói ở trên, quá trình thử - sai ban đầu được xem xét ở mức đơn giản hơn. Cụ thể,
trong bài toán này, thay vì xem xét việc tìm kiếm chuỗi nước đi phủ khắp bàn cờ, ta xem xét vấn
đề đơn giản hơn là tìm kiếm nước đi tiếp theo của quân mã, hoặc kết luận rằng không còn nước đi
kế tiếp thỏa mãn. Tại mỗi bước, nếu có thể tìm kiếm được 1 nước đi kế tiếp, ta tiến hành ghi lại
nước đi này cùng với chuỗi các nước đi trước đó và tiếp tục quá trình tìm kiếm nước đi. Nếu tại
bước nào đó, không thể tìm nước đi kế tiếp thỏa mãn yêu cầu của bài toán, ta quay trở lại bước trước,
 hủy bỏ nước đi đã lưu lại trước đó và thử sang 1 nước đi mới. Quá trình có thể phải thử rồi
quay lại nhiều lần, cho tới khi tìm ra giải pháp hoặc đã thử hết các phương án mà không tìm ra
giải pháp.
* Đầu tiên, ta sử dụng 1 mảng 2 chiều đề mô tả bàn cờ:
Các phần tử của mảng này có kiểu dữ liệu số nguyên. Mỗi phần tử của mảng đại diện cho 1
ô của bàn cờ. Chỉ số của phần tử tương ứng với tọa độ của ô, chẳng hạn phần tử Banco[0][0]
tương ứng với ô (0,0) của bàn cờ. Giá trị của phần tử cho biết ô đó đã được quân mã đi qua hay
chưa. Nếu giá trị ô = 0 tức là quân mã chưa đi qua, ngược lại ô đã được quân mã đã đi qua.
* Các phần tử của mảng này có kiểu dữ liệu số nguyên. Mỗi phần tử của mảng đại diện cho 1
ô của bàn cờ. Chỉ số của phần tử tương ứng với tọa độ của ô, chẳng hạn phần tử Banco[0][0]
tương ứng với ô (0,0) của bàn cờ. Giá trị của phần tử cho biết ô đó đã được quân mã đi qua hay
chưa. Nếu giá trị ô = 0 tức là quân mã chưa đi qua, ngược lại ô đã được quân mã đã đi qua.
* Banco[x][y] = 0: ô (x,y) chưa được quân mã đi qua
* Banco[x][y] = i: ô (x,y) đã được quân mã đi qua tại nước thứ i.
* Nước đi kế tiếp chấp nhận được nếu nó chưa được quân mã đi qua, tức là nếu ô (u,v) được
chọn là nước đi kế tiếp thì Banco[u][v] = 0 là điều kiện để chấp nhận. Ngoài ra, hiển nhiên là ô đó
phải nằm trong bàn cờ nên 0 ≤ u, v < n.
* Việc ghi lại nước đi tức là đánh dấu rằng ô đó đã được quân mã đi qua. Tuy nhiên, ta cũng
cần biết là quân mã đi qua ô đó tại nước đi thứ mấy. Như vậy, ta cần 1 biến i để cho biết hiện tại
đang thử ở nước đi thứ mấy, và ghi lại nước đi thành công bằng cách gán giá trị Banco[u][v]=i.
* Do i tăng lên theo từng bước thử, nên ta có thể kiểm tra xem bàn cờ còn ô trống không bằng
cách kiểm tra xem i đã bằng n 2 chưa. Nếu i<n 2 tức là bàn cờ vẫn còn ô trống.
* Theo luật cờ thông thường, quân mã từ ô (x,y) có thể đi tới 8 ô trên bàn cờ như trong hình vẽ:

![Cac nuoc di cua quan ma](capture2.png)

* Ta thấy rằng 8 ô mà quân mã có thể đi tới từ ô (x,y) có thể tính tương đối so với (x,y) là:
(x+2, y-1); (x+1, y-2); (x-1, y-2); (x-2, y-1); (x-2, y+1); (x-1, y+2); (x+1; y+2); (x+2, y+1)
* Nếu gọi dx, dy là các giá trị mà x, y lần lượt phải cộng vào để tạo thành ô mà quân mã có
thể đi tới, thì ta có thể gán cho dx, dy mảng các giá trị như sau:
* dx = {2, 1, -1, -2, -2, -1, 1, 2}
* dy = {-1, -2, -2, -1, 1, 2, 2, 1}
* Như vậy, danh sách các nước đi kế tiếp (u, v) có thể được tạo ra như sau:
u = x + dx[i]
v = y + dy[i]
* với các nước đi như trên thì (u, v) có thể là ô nằm ngoài bàn cờ. Tuy nhiên, như
đã nói ở trên, ta đã có điều kiện 0 ≤ u, v < n, do vậy luôn đảm bảo ô (u, v) được chọn là hợp lệ.
## Các bước lấy dữ liệu để hiển thị thuật toán 
* Từ thuật toán đệ quy quay lui: 
```javascript
let canMove = (x, y) => {
            return x >= 0 && x < size && y >= 0 && y < size && board[x][y] === 0;
        };
        let tour = (x, y, step) => {

            if (step === totalSpace) {
                board[x][y] = step;
                return true;
            }
            board[x][y] = step;

            if (canMove(x - 1, y - 2) && tour(x - 1, y - 2, step + 1)) {
                return true;
            }

            if (canMove(x - 2, y - 1) && tour(x - 2, y - 1, step + 1)) {
                return true;
            }
            if (canMove(x - 2, y + 1) && tour(x - 2, y + 1, step + 1)) {
                return true;
            }

            if (canMove(x - 1, y + 2) && tour(x - 1, y + 2, step + 1)) {
                return true;
            }

            if (canMove(x + 1, y + 2) && tour(x + 1, y + 2, step + 1)) {
                return true;
            }

            if (canMove(x + 2, y + 1) && tour(x + 2, y + 1, step + 1)) {
                return true;
            }

            if (canMove(x + 2, y - 1) && tour(x + 2, y - 1, step + 1)) {
                return true;
            }

            if (canMove(x + 1, y - 2) && tour(x + 1, y - 2, step + 1)) {
                return true;
            }

            board[x][y] = 0;

            //return false;
        };

```
* Sẽ cho ra 1 ma trận chứa các nước đi như sau:
```javascript
31 02 17 24 33 36 
16 07 32 35 18 25 
03 30 01 26 23 34 
08 15 06 21 12 19 
29 04 13 10 27 22 
14 09 28 05 20 11 
```
* Từ ma trận kết quả trên ta thấy tương ứng với nước đi đầu tiên (01) ở  hàng 2 cột 2 tương ứng với ô (2,2) trên bàn cờ.
* Do vậy ta có thể chuyển ma trận các nước đi này về dạng 1 mảng object bao gồm các thuộc tính: object = { nuocdi, x, y}
* Sau đó ta tiến hành sắp xếp mảng này theo thứ tự tăng dần của nước đi
* Sau khi có mảng các tọa độ của các nước đi ta tiến hành vẽ hành trình của quân mã bằng d3.js với dữ liệu đầu vào là mảng Arr_obj chứa các đối tượng
* Code để vẽ như sau: 
```javascript
 let drawBoard = () => {
            const boxSize = 80,
                boardDimension = N,
                boardSize = boardDimension * boxSize
            const div = d3.select("#svg-container");
            const svg = div.append("svg")
                .attr("width", boardSize + "px")
                .attr("height", boardSize + "px");
        <---------------vẽ bàn cờ---------------------->
            for (let i = 0; i < boardDimension; i++) {
                for (let j = 0; j < boardDimension; j++) {
                    // draw each chess field
                    const box = svg.append("rect")
                        .attr("x", i * boxSize)
                        .attr("y", j * boxSize)
                        .attr("width", boxSize + "px")
                        .attr("height", boxSize + "px")
                    if ((i + j) % 2 === 0) {
                        box.attr("fill", "beige");
                    } else {
                        box.attr("fill", "gray");
                    }
                }
            }
    <-------vẽ quân cờ ở vị trí bắt đầu và số thự các nước đi------->

            for (let i = 0; i < Arr_obj.length; i++) {
                  const chess = svg.append("text")
                    .style("font-size", boxSize / 6)
                    .attr("text-anchor", "middle")
                    .attr("x", Arr_obj[i]._x * boxSize + boxSize/2)
                    .attr("y", Arr_obj[i]._y * boxSize + boxSize/2)
                    .style("text-shadow", "2px 2px 4px #757575")
                    .classed('team1', true)
                if ((i === 0)) {
                    chess.style("font-size", boxSize * 2 / 4)
                    chess.text(knight.b)
                    chess.attr("id", "ma")

                }
                else {
                    chess.text(i)
                }
            }
<------------------kết thúc vẽ bàn cờ------------------->
<------------------Vẽ hành trình của quân cờ------------->
            
            var bezierLine = d3.svg.line()
                .x(function (d) {
                    return d._x * boxSize + boxSize/2;
                })
                .y(function (d) {
                    return d._y * boxSize + boxSize/2;
                })
                .interpolate("linear");
            svg.append('path').attr("d", bezierLine(Arr_obj)).attr("stroke", "black")
                .attr("stroke-width", 2)
                .attr("fill", "none")
                .transition()
                .duration(function () {
                    for (let i = 1; i < Arr_obj.length; i++) {
                        return i * 40000;
                    }
                })
                .attrTween(
                    "stroke-dasharray",
                    function () {
                        var len = this.getTotalLength();
                        return function (t) {
                            return (d3.interpolateString("0," + len, len + "," + 10 * len))(t)
                        };
                    });
        }
```
## Hành trình của quân mã với n=6 tọa độ ban đầu (x,y)=(2,2)

![Cac nuoc di cua quan ma](Screenshot from 2017-06-01 09-14-04.png)























