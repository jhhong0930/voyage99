flask에서 db에 있는 데이터 조회 후 넘길때

```python
@app.route('/review', methods=['GET'])
def read_review():
   isbn = request.args.get('isbn')
   reviews = list(db.booktable.find({'isbn': isbn}))
   print(isbn, len(reviews))
   return jsonify({'reviews': reviews})
```

해당 방식으로 데이터를 넘기게 되면 TypeError: Object of type ObjectId is not JSON serializable 에러가 발생한다

이럴경우 dumps() 함수를 이용하여 json으로 변환 해준다

```python
return jsonify({'reviews': dumps(reviews)})
```

데이터를 받아서 ajax로 처리

```html
    function show_review() {
        let isbn = {{isbn}}
        $.ajax({
            type: "GET",
            url: `/review?isbn=${isbn}`,
            data: {},
            success: function (response) {
                // response[넘겨준 데이터 키] -> JSON.parse로 형변환
                let reviews = JSON.parse(response['reviews'])

                // 컬럼 접근: reviews[행][컬럼명]
                for (let i=0; i<reviews.length; i++) {
                    console.log(reviews[i]['review']);
                    console.log(reviews[i]['isbn']);
                }
            }
        });
    }
```

