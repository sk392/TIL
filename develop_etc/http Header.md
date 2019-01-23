## http Header

Http 헤더에는 한글이 들어갈 수 없다. 끝

Http Header에는 ASCII(American Standard Code for Information Interchange)만 들어갈 수 있고, Unicode는 들어갈 수 없다. 떄문에 Header의 Key,Value를 넣을 때 한글이 필요하다면 Base64나 Urlencoding이 필요하게 된다.

이런 정의를 할 땐 서버와 상의해서!