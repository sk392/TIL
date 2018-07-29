## Android Mp3 파일 찾기

오늘은 넘나 일하기가 싫어서.. 간단한걸 정리해본당..

1. 루트를 기준으로 해당 파일 리스트를 가져온다
2. 해당 파일이 폴더일 경우 (Directory) 해당 폴더 루트를 기준으로 파일리스트를 다시 가져온다.
3. 파일일 경우 마지막이 .mp3로 끝나는지 확인한다
4. mp3파일이면 해당 경로를 기준으로 파일 이름을 해쉬에 저장한다.
5. ArrayList는 파일 경로를 기준으로 파일들을 리스트로 가지게 된다.



```
ArrayList<HashMap<String,String>> getPlayList(String rootPath) {
            ArrayList<HashMap<String,String>> fileList = new ArrayList<>();


            try {
                File rootFolder = new File(rootPath);
                File[] files = rootFolder.listFiles(); //here you will get NPE if directory doesn't contains  any file,handle it like this.
                for (File file : files) {
                    if (file.isDirectory()) {
                        if (getPlayList(file.getAbsolutePath()) != null) {
                            fileList.addAll(getPlayList(file.getAbsolutePath()));
                        } else {
                            break;
                        }
                    } else if (file.getName().endsWith(".mp3")) {
                        HashMap<String, String> song = new HashMap<>();
                        song.put("file_path", file.getAbsolutePath());
                        song.put("file_name", file.getName());
                        fileList.add(song);
                    }
                }
                return fileList;
            } catch (Exception e) {
                return null;
            }
        }
```