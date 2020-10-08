# cv,iv초기화 방법
1. 자동 초기화 : 0값으로
2. 간단 초기화 : 대입연산자(=)사용
3. 복잡한 초기화 : { } , static{ } , 생성자
    - static{ }은 cv초기화
    - 생성자는 iv초기화
    - { }도 iv초기화지만 거의 사용되지 않음


# cv , iv
- iv : 인스턴스변수
- cv : 클래스변수 = 공유변수

public class test {

	int iv; // 인스턴스 변수
	static int cv; // 클래스 변수
	
	void method() {
		int lv; // 지역 변수
	}
}

![https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile28.uf.tistory.com%2Fimage%2F21226F42578D2F8137324A]