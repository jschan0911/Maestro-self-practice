# 마에스트로 API 명세 작성 현황

## 🙂 유저 관련
- [ ] 회원 가입 : 아이디, 비밀번호, 이메일, 닉네임 입력 [(논의 필요)](#이런-내용은-논의해보고-싶습니다)
  - [ ] 이메일 형식 맞는지 검증
  - [ ] 이메일 중복인지 검증 (미정)
  - [ ] 이메일 인증 (미정)
  - [ ] 비밀번호 조건 부합하는지 검증 (미정)
  - [ ] 닉네임에 비속어 포함되었는지 여부 검증 (미정)
  - [ ] 닉네임 중복 여부 검증 (미정)
<pre><code>POST: /user/signup
# Request
{ 
  "userID" : "inhaKim2022",
  "password1" : "@qwer1234",
  "password2" : "@qwer1234",
  "nickname" : "김인하",
  "userEmail" : "inhaKim2022@naver.com"
}
</code></pre>
- [ ] 로그인
  - [ ] 로그인 시 접속에 관한 booleanField 활성화
<pre><code>POST: /user/login
# Request
{
  "userID" : "inhaKim2022",
  "password" : "@qwer1234"
}
</code></pre>

## 👥 팀 관련
- [ ] 팀 생성 : 팀 이름, 팀을 설명할 사진 (프사) 입력
<pre><code>POST: /team
# Request
{
  "teamName" : "멋쟁이 사자 밴드",
  "teamImage" : "../../사진파일.JPG"  // 형식 모르겠음 ㅎㅎ
}
</code></pre>
- [ ] 팀 수정
<pre><code>PUT: /team/{teamID}
# Request
{
  "teamName" : "멋쟁이 사자 밴드 수정 버전",
  "teamImage" : "../../사진파일.JPG"
}
</code></pre>
- [ ] 팀 삭제
<pre><code>DELETE: /team/{teamID}</code></pre>
- [ ] 팀 우선 순위 설정 (별 표시 클릭시 위 쪽으로 나오게)
  - [ ] 소속 팀 테이블에서 제거
  - [ ] 중요 팀 테이블에 추가
<pre><code>POST : /team/priority/{teamIO}
</code></pre>
- [ ] 신규 팀원 가입 시
  - [ ] 총 인원 수 추가
  - [ ] 유저 모델 내 소속 팀 테이블에 추가
<pre><code>POST: /user/team/{teamID}</code></pre>
- [ ] 기존 팀원 탈퇴
  - [ ] 총 인원 수 감소
  - [ ] 유저 모델 내 소속 팀 or 중요 팀 테이블에서 제거
<pre><code>DELETE: /user/team/{teamID}</code></pre>
#### 약간 유저 관련? (유저 모델 내 팀 테이블이 ManyToMany 연결 관계라 유저와 관련이 있다고 생각했음)
- [ ] 유저가 속해있는 팀 목록 조회
  - [ ] 어떤 팀 페이지에 접속해있는지 판별 : 접속한 팀에 관한 정보(pk?)를 불러와 저장 [(논의 필요)](#이런-내용은-논의해보고-싶습니다)
  - [ ] 팀에 속한 팀원 중, 현재 팀 페이지 이하에 접속 중인 인원 수 계산 [(논의 필요)](#이런-내용은-논의해보고-싶습니다)
<pre><code>GET: /team
# Response
{
  "specialTeams" : [
    {
      "id" : 3,
      "teamName" : "인천광역시 밴드"
      "teamImage" : "../../사진파일3.JPG",
      "connectedPeopleNumber" : 3,
      "totalPeopleNumber" : 7
    }
  ],
  "normalTeams" : [
    {
      "id" : 1,
      "teamName" : "멋쟁이 사자 밴드",
      "teamImage" : "../../사진파일.JPG"
      "connectedPeopleNumber" : 5,
      "totalPeopleNumber" : 5
    },
    {
      "id" : 4,
      "teamName" : "인하대학교 밴드부"
      "teamImage" : "../../사진파일4.JPG"
      "connectedPeopleNumber" : 2,
      "totalPeopleNumber" : 6
    }
  ]
}

//논의 필요 : 페이지 접속 시 접속 팀에 대한 정보를 불러와 저장하는 작업은 GET이 아님!</code></pre>
- [ ] 팀에 유저를 초대 [(논의 필요)](#이런-내용은-논의해보고-싶습니다)
  - [ ] 초대하는 링크 생성
  - [ ] 해당 링크 접속한 유저를 팀에 가입 시킴
<pre><code>POST: /user/team/{teamID}  //신규 팀원 가입 URL과 같음 (똑같은 기능)</code></pre>

## 📚 콘티(폴더) 관련
- [ ] 폴더 생성 : 폴더 이름, 폴더를 설명할 사진 (프사), 어떤 팀의 폴더인지에 대한 정보 입력
  - [ ] 폴더 생성 시, 어떤 팀의 폴더인지를 나타내는 테이블에 자동으로 {teamID} 추가 (사용자가 입력하지 않아도)
<pre><code>POST: /folder/{teamID}
# Request
{
  "folderName" : "동아리 축제 공연 노래 모음",
  "folderImage" : "../../사진파일10.JPG",
}</code></pre>
- [ ] 폴더 수정
<pre><code>PUT: /folder/{teamID}/{folderID}
# Request
{
  "folderName" : "동아리 축제 공연 노래 모음",
  "folderImage" : "../../사진파일11.JPG",
}</code></pre>
- [ ] 폴더 삭제
<pre><code>DELETE: /folder/{teamID}/{folderID} </code></pre>
- [ ] 특정 팀이 보유한 폴더 목록 보여줌
<pre><code>GET: /folder/{teamID} 
# Response
{
  "folderLists" : [
    {
      "id" : 1,
      "folderName" : "동아리 축제 공연 노래 모음",
      "folderImage" : "../../사진파일11.JPG",
      "teamID" : 13
    },
    {
      "id" : 3,
      "folderName" : "11월 14일 버스킹",
      "folderImage" : "../../사진파일13.JPG",
      "teamID" : 13
    },
    {
      "id" : 6,
      "folderName" : "전국 대학 밴드 경진대회",
      "folderImage" : "../../사진파일16.JPG",
      "teamID" : 13
    },
    {
      "id" : 7,
      "folderName" : "외부 축제 공연",
      "folderImage" : "../../사진파일17.JPG",
      "teamID" : 13
    }
  ]
}</code></pre>

## 📖 곡 관련
- [ ] 곡 생성 : 곡 이름, BPM 정보, 악보 파일, 어떤 폴더의 곡인지에 대한 정보 입력
  - [ ] 곡 생성 시, 어떤 폴더의 곡인지를 나타내는 테이블에 자동으로 {folderID} 추가 (사용자가 입력하지 않아도)
<pre><code>POST: /music/{folderID}
# Request
{
  "musicName" : "사건의 지평선",
  "musicBPM" : 98,
  "sheetMusic" : "../../사진파일31.JPG" //악보
}</code></pre>
- [ ] 곡 수정
<pre><code>PUT: /music/{folderID}/{musicID}
# Request
{
  "musicName" : "사건의 지평선",
  "musicBPM" : 100,
  "sheetMusic" : "../../사진파일31.JPG"
}</code></pre>
- [ ] 곡 삭제
<pre><code>DELETE: /music/{folderID}/{musicID}</code></pre>
- [ ] 특정 폴더 내 곡 목록 보여줌
<pre><code>GET: /music/{folderID}
# Response
{
  "musicLists" : [
    {
      "id" : 8,
      "musicName" : "사건의 지평선",
      "musicBPM" : 100,
      "sheetMusic" : "../../사진파일31.JPG",
      "folderID" : 2
    },
    {
      "id" : 10,
      "musicName" : "별 보러 가자",
      "musicBPM" : 70,
      "sheetMusic" : "../../사진파일32.JPG",
      "folderID" : 2
    },
    {
      "id" : 11,
      "musicName" : "그라데이션",
      "musicBPM" : 105,
      "sheetMusic" : "../../사진파일33.JPG",
      "folderID" : 2
    }
  ]
}</code></pre>

## 🎶 팀과 공유하는 메트로놈 관련
- [ ] 동기화를 적용할 시간을 설정함 [(논의 필요)](#이런-내용은-논의해보고-싶습니다)

---

# 수정 / 추가 작업이 필요할 것 같습니다!

- [ ] 팀 수정 버튼 및 페이지가 필요할 것 같습니다.
- [ ] 팀 클릭 시 이동할 '콘티 (폴더) 모음 페이지'가 필요할 것 같습니다.
- [ ] 콘티 수정 버튼 및 페이지가 필요할 것 같습니다.
- [ ] 동기화를 적용할 시간을 설정하는 페이지가 필요할 것 같습니다. (알람 설정 느낌)
- [ ] 팀 삭제 시 본인 유저 모델 내 소속 팀 or 특수 팀 테이블에서 삭제하는 것인지, 팀 모델 자체를 삭제하는 것인지 구분하는 페이지가 필요할 것 같습니다.

---

# 이런 내용은 논의해보고 싶습니다!

- 초대 링크에 접속한 어떤 유저든 가입할 수 있도록 처리할 것인가요? (이건 일단 해봐야 알 것 같긴 합니다만..)
- 현재 접속 중인 수를 세는 이유가 원활한 합주를 진행하기 위함이라면, </br>
  팀 페이지에 접속한 유저를 카운트하기 보다는 </br>
  콘티(폴더) 페이지에 접속한 유저를 카운트 하는 건 어떨까요? </br></br>
  왜냐하면 팀 안에도 여러 개의 콘티(폴더)가 있을 텐데, </br>
  합주하는 팀원 중 몇 명이 같은 콘티(폴더)를 보고 있는지에 대한 정보가 궁금할 것 같기 때문입니다.
- [팀과 공유하는 메트로놈 관련](#🎶-팀과-공유하는-메트로놈-관련) 내 '동기화를 적용할 시간을 설정'하는 작업은 언제 적용하는 것이 좋을까요?
- [회원가입 할 때](#🙂-유저-관련) 조건들을 추가해보는 건 어떨까요?
  - 회원가입시 이메일로 인증
  - 회원가입시 비밀번호 설정할 때 조건 설정 (특수번호, 몇 자리수 이상, ...)
  - 닉네임 설정 중, 비속어 필터링, 중복 필터링 ...