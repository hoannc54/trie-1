# Hướng dẫn về Trie và các ví dụ

Tôi sẽ viết trong bài này rằng Tries và các khái niệm được sử dụng trong các thao tác nhỏ của vấn đề. Chúng ta sẽ xem 2-3 vấn đề mà trie sẽ hữu ích.

Đầu tiên chúng ta sẽ xem trie là gì. Trie có thể lưu trữ thông tin về keys/numbers/strings (khóa/số/chuỗi) nhỏ gọn trong một cây.

Tries bao gồm các nút, mỗi một nút khác nhau lưu trữ một character(kí tự)/bit. Chúng ta có thể thêm string/numbers (chuỗi/số) mới phù hợp.
Đây là ví dụ trie về strings(chuỗi)

![][1]

  
Nguồn: Wikipedia.

Nhưng chúng ta sẽ xử lý các numbers (con số) ở đây, và đặc biệt trong các bit nhị phân. Chúng ta sẽ thấy khi chúng ta giải quyết các vấn đề. 

**Vấn đề 1**: Cho một mảng số nguyên, chúng ta phải tìm 2 phần tử mà XOR là lớn nhất 
**Giải pháp:**  
Giả sử chúng ta có 1 cấu trúc dữ liệu mà có thể đáp ứng 2 kiểu try vấn:
1\. Thêm một số X  
2\. Cho a Y, tìm giá trị lớn nhất XOR của Y với tất cả các số mà đã được thêm đến bây giờ.
Nếu chúng ta có cấu trúc dữ liệu này, chúng ta sẽ thêm số nguyên. và với truy vấn kiểu 2, chúng ta có thể tìm được giá trị lớn nhất XOR.
Trie là cấu trúc dữ liệu chúng ta sẽ sử dụng. Đầu tiên, hãy xem làm thế nào để thêm các phần tử vào trong trie.

![][2]

Vậy, chúng ta theo đường dẫn của các con số mà chúng ta cần chèn, chúng ta không phải vẽ lại đường dẫn hiện có.

Chèn một key có độ dài N cần O(N) là log2(MAX) trong đó log2(MAX) là số lớn nhất được chèn vào trie, bởi vì có tối đa log2(MAX) bit nhị phân trong một số.
 
Bằng cách này, chúng ta lưu trữ tất cả dữ liệu về tất cả các số được chèn vào trie cho đến bây giờ.

Bây giờ, cho câu truy vấn kiểu 2:   

Giả sử số Y của chúng ta là b1, b2, ..., bn, trong đó b1, b2, ... là các số nhị phân. Chúng ta bắt đầu từ b1. Bây giờ cho XOR là lớn nhất, chúng ta sẽ cố gắng tạo ra một bit quan trong sau khi dùng XOR. Vậy, nếu b1 là 0, chúng ta cần 1 và ngược lại. Trong một trie, chúng ta sẽ đi đến vùng bit được yêu cầu. Nếu tùy chọn thuận lợi không có, chúng ta sẽ chuyển sang vùng khác. Thực hiện cách này với i = 1 tới n, chúng ta sẽ lấy được giá trị lớn nhất của XOR.

![][3]

Câu truy vấn là log2(MAX)

**Vấn đề 2**: Cho một mảng các số nguyên, tìm mảng con với giá trị lớn nhất XO.
**Giải pháp:**  
Giả sử F(L,R) là mảng con của XOR từ L đến R.
Ở đây chúng ta sử dụng tính chất F(L,R) = F(1,R) XOR F(1,L-1). Làm thế nào?
Giả sử mảng con với XOR lớn nhất kết thúc tại vị trí i. Bây giờ, chúng ta cần tối đa F(L,i) ie. F(1,i) XOR F(1,L-1) khi L<=i. Giả sử, chúng ta đã chèn F(1,L-1) trong trie cho mọi L<=i, lúc đó bài toán trở về vấn đề 1.
    
    
    ans=0
    pre=0
    Trie.insert(0)
    for i=1 to N:
        pre = pre XOR a[i]
        Trie.insert(pre)
        ans=max(ans, Trie.query(pre))
    print ans
    

Bạn có thể thử vấn đề ở đây: [ACM-ICPC Live Archive][4]

**Vấn đề 3**: Cho một mảng các số nguyên dương, bạn phải in các số lượng các mảng con mà XOR nhỏ hơn K
**Giải quyết:**  
Giải pháp này một lần nữa sử dụng các khái niệm mà ta đã thấy cho đến bây giờ. Chúng ta sẽ giải quyết như câu hỏi trước đó.
Đối với mỗi chỉ số từ i từ 1 đến N, chúng ta có thể đếm xem bao nhiêu mảng con kết thúc tại vị trí thứ i mà thỏa mãn điều được được đưa ra.


    
    
    ans=0
    p=0
    for i=1 to N:
        q=p XOR A[i]
        ans += Trie.query(q,k)
        Trie.insert(q)
        p=q
    

  
query(q,k) trả về các số nguyên đã tồn tại trong câu trúc mà khi lấy XOR với q trả về một số nguyên hơn k.
Chúng ta so sách các bit tương ứng của q và k, bắt đầu từ các bit quan trọng nhất. Giả sử p và q là các bit tương ứng mà chúng ta đang xem xét.

Nếu q là 1, và p là 0, thì chúng ta thực hiện điều này:

![][5]

Tương tự chúng ta có thể dễ dàng làm ra với 3 trường hợp khác. (q=1,p=1), (q=0,p=1) và (q=0,p=1).

Vì vậy, chúng ta cần phải thay đổi cấu trúc của chúng ta ở đây, chúng ta cũng giữ một số lượng các nút lá mà có thể tiếp tận từ nút hiện tại nếu đi từ phía bên trái và tương tự với phái bên phải. Bởi vì, nếu không, sự phức tạp sẽ tăng lên, nếu chúng ta duyệt cây từ lần này đến lần khác. Chúng ta có thể làm điều này trong khi thêm số vào trong cây một cách dễ dàng.

Vấn đề này đã được nêu tại CodeCraft'14. Bạn có thể thực hành tại đây [SPOJ.com - Problem SUBXOR][6]

Bây giờ, hãy nói về việc triển khai.
Để triển khai code cho trie trong C/CPP chúng ta có thể giữ các nút và các con trỏ trái và phải. Chúng ta có thể viết hàm đệ quy.


    
    
    insert(root, num, level):
        if level==-1: return root
        x=level'th bit of num
        if x==1:
            if root->right is NULL: create root->right
            else: insert(root->right,num,level-1)
        else:
            if root->left is NULL: create root->left
            else: insert(root->left,num,level-1)
    

  
Đối với các truy vấn cũng vây, ta cần duyệt đệ quy trên cây.

Cập nhật: 
Một vấn đề khác khi sử dụng Trie(yay! :P).  
[Vấn đề - B - Codeforces][7]

**Vấn đề con**: Cho một nhóm gồm n chuỗi không rỗng. Trong trò chơi, 2 người cùng xây dựng "từ" cùng nhau,  khởi đầu thì "từ" rỗng. Các người chơi theo lượt. Trong lượt của mình, người chơi phải thêm chữ cái vào cuối của "từ", kết quả của "từ" phải là tiền tố của ít nhất một chuỗi từ nhóm. Người chơi thua nếu họ không thể thực hiên bước đi của mình.

Chúng ta cần tìm người chơi nào (đầu tiên hoặc thứ 2) mà có khả năng chiến thắng.

Vì vậy, ý tưởng ở đây lại là xây dựng một tri với tất cả các chuỗi. Tại sao? Bởi vì một trie lưu trữ thông tin về tất cả các tiền tố.
Bây giờ, chúng ta sẽ thử đánh giá xem với mỗi nút khác nhau liệu người chơi đầu tiên có khả năng chiến thắng hoặc không. Chúng ta có thể làm điều này với đệ quy. Với các nút v, từng nút u mà u là con trực tiếp của v, nếu người chơi đầu tiên có khả năng thua vì u, thì với nút v người chơi đầu tiên có khả năng dành chiến thắng.
Ví dụ, giả sử ta có "abc", "abd", "acd".
Trie của chúng ta sẽ nhìn như sau:


![][8]

  
Tất cả các nút lá đều có khả năng chiến thắng.

[1]: https://qph.fs.quoracdn.net/main-qimg-aea28d9cd34aaf2d5783f4cd04e5abbd
[2]: https://qph.fs.quoracdn.net/main-qimg-388217a1992f1b2aac51e9917aa76d9c
[3]: https://qph.fs.quoracdn.net/main-qimg-e5d624e2cd693d713840a30ca9aaa461
[4]: https://icpcarchive.ecs.baylor.edu/index.php?Itemid=8&category=345&option=com_onlinejudge&page=show_problem&problem=2683
[5]: https://qph.fs.quoracdn.net/main-qimg-f24ea5ecf11805e7bcd82a48bb9cad25
[6]: http://www.spoj.com/problems/SUBXOR
[7]: http://codeforces.com/contest/455/problem/B
[8]: https://qph.fs.quoracdn.net/main-qimg-f81def67dffcc9e95306d65b27daa2f7-c
