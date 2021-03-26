# Try-with-resources

> 자원을 쉽게 해제하기
>
> try에서 선언된 객체들이 try 종료될 때 자동으로 자원을 해제해주는 기능 (`close`)
>
> Java7 부터 지원



- `try-catch-finally` 로 해제

~~~java
public static void main(String args[]) throws IOException {
    FileInputStream is = null;
    BufferedInputStream bis = null;
    try {
        is = new FileInputStream("file.txt");
        bis = new BufferedInputStream(is);
        int data = -1;
        while((data = bis.read()) != -1){
            System.out.print((char) data);
        }
    } finally {
        // close resources
        if (is != null) is.close();
        if (bis != null) bis.close();
    }
}
~~~

> `finally`에서 자원들을 close해줘야하는 불편함이 발생!



- `try-with-resource` 사용

~~~java
public static void main(String args[]) {
    try (
        FileInputStream is = new FileInputStream("file.txt");
        BufferedInputStream bis = new BufferedInputStream(is)
    ) {
        int data = -1;
        while ((data = bis.read()) != -1) {
            System.out.print((char) data);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
~~~

> `try`문을 벗어나면 `try` 안에서 선언된 객체의 `close()` 메소드를 호출
>
> ▶ close()를 명시적으로 호출해줄 필요가 없다