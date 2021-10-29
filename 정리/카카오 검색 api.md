### 카카오 검색 api를 이용하여 도서 검색 활용

```javascript
    function get_list() {
        $('#book-box').empty();
        let book_name = $("#book_name").val(); // 검색 키워드
        $.ajax({
            type: "GET",
            url: "https://dapi.kakao.com/v3/search/book?target=title",
            data: { // 검색 조건 지정
                query: book_name, 
                size: 50
            },
            headers: {Authorization: "KakaoAK REST_API_KEY"},
        }).done(function (msg) {
            for (let i=0; i<msg.documents.length; i++) {
                console.log(msg)
                // 각 타입은 document[행 번호].타입 으로 접근할 수 있다
                let title = msg.documents[i].title;
                let thumbnail = msg.documents[i].thumbnail;
                let contents = msg.documents[i].contents;
                // 상세 페이지로 이동시 해당 책의 정보를 넘겨야하는데 isbn을 이용
                // isbn을 조회하면 'ISBN10 ISBN13' 이 나오는데 이중 ISBN13을 이용
                let isbn = msg.documents[i].isbn.substring(11)

                let temp_html =
                    `
                    <div class="card" style="width: 18rem;">
                        <a href="detail?isbn=${isbn}">
                        <img src="${thumbnail}" class="card-img-top" alt="...">
                        </a>
                        <div class="card-body">
                            <h5 class="card-title">${title}</h5>
                            <p class="card-text">${contents}</p>
                         </div>
                    </div>
                    `
                $('#book-box').append(temp_html)
            }
        })
    }
```

```javascript
// 상세보기
    function get_list() {
        // 검색 페이지 요청시 쿼리 파라미터로 넘긴 isbn 가져오기
        let isbn = {{isbn}}
        $.ajax({
            type: "GET",
            url: "https://dapi.kakao.com/v3/search/book?target=title",
            data: {
                // 가져온 isbn으로 도서 정보 조회
                query: isbn
            },
            headers: {Authorization: "KakaoAK REST_API_KEY"},
        }).done(function (msg) {
			// 생략
        })
    }
```

