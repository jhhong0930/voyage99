회원 정보 수정시 닉네임과 이미지가 들어오는데, 프론트에서 이미지는 선택을 안하고 닉네임만 넘기면 오류가 발생했었다

```java
// dto
private string nickname;
private MultipartFile image;

// 회원 정보 수정
@Transactional
public void updateUser(UserDetailsImpl userDetails, UpdateUserRequestDto requestDto) throws IOException {

    User user = userRepository.findById(userDetails.getUser().getId()).orElseThrow(
        () -> new NullPointerException("존재하지 않는 회원입니다")
    );

    // 닉네임을 변경하지 않아도 dto에 값이 들어오게 되는데 현재 닉네임과 다를 경우에만 수정
    if (!requestDto.getNickname().equals(user.getNickname())) {

        Optional<User> foundNickname = userRepository.findByNickname(requestDto.getNickname());

        if (foundNickname.isPresent()) {
            throw new IllegalArgumentException("이미 사용중인 닉네임 입니다");
        }
    }

    String imageUrl = user.getImage();

    if (requestDto.getImage() != null) {

        deleteS3(user.getImage());

        imageUrl = s3Uploader.upload(requestDto.getImage(), "userImage");

        if (imageUrl == null) throw new IllegalArgumentException("이미지 업로드에 실패하였습니다");
    }

    user.changeProfile(requestDto.getNickname(), imageUrl);
}
```

postman으로 테스트할때는 이미지를 선택하지 않으면 null로 값이 들어왔는데, 프론트에서 이미지에 대한 value를 null로 보내려고하니 서버쪽 콘솔에서 String 타입을 Multipart로 변환할 수 없다는 오류가 발생하였다.

Multipart/form-data로 전송 시 파일 업로드를 하지 않으면 오류가 발생, null로 보내도 String으로 인식하여 처음에는 닉네임만 변경하는 API를 새로 설계했다가 이미지는 수정하지 않을시 값 자체를 넘기지 않도록 바꾸니 오류가 해결되었다