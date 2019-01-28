## android_edittext with samsung keyboard

삼성 안드로이드 특정 기기 (갤럭시노트8(7.1.1), 갤럭시 S7 (7.0), 갤럭시s9(8.0))에서 ``` editText.text.clear()``` 를 하는 경우 텍스트가 지워지고 textWatcher에도 ""가 들어오는데, 이때 한번 더 문자를 입력하는 경우 지워지기 전의 문자열 + 입력한 문자열로 입력이 들어온다 (textWatcher의 리스너로 들어오는 것도 동일) 

 추론하기론 삼성키보드(쿼티)에만 발생하는데 이 안에서 자동완성이나 추천 단어 등을 위해서 editText와 별도로 텍스트를 가지고 있는 것으로 보이고, ``` text.clear() ``` 대신에 ``` editText.text = "" ``` 로 하면 해결된다.
