문제

<img width="1258" height="455" alt="image" src="https://github.com/user-attachments/assets/ff330f05-25bf-4a0e-b5db-11d41d4409a6" />


문제풀이

이번 문제는 html로 되어 있고 자바스크립트 관련 내용을 풀어야 하는 것으로 보인다..
<img width="878" height="288" alt="image" src="https://github.com/user-attachments/assets/77919338-170e-4a20-a82a-8e0f8d7a726a" />


어질어질하게 난독화 되어 있는 모습으로 확인
<img width="3802" height="1278" alt="image" src="https://github.com/user-attachments/assets/b6771aa2-c6d1-4a5d-8b88-7d673b5138f0" />


생년월일 6자리를 입력하고 버튼클릭 시 _0x9a220(생년월일)로 기능이 실행되는 것을 확인할 수 있다.

<img width="1429" height="115" alt="image" src="https://github.com/user-attachments/assets/93eac16f-64f1-4ada-8c5f-9763a49f8df3" />


생년월일을 잘못 입력하면 alert("Wrong")
올바른 생년월일을 입력하면 document.write로 페이지에 정답이 쓰여질 것으로 판단됩니다.
<img width="2012" height="421" alt="image" src="https://github.com/user-attachments/assets/9590a272-372a-49c3-b9a2-d77c521b5af6" />


난독화를 시도하였지만, 너무 어렵고 할 수 없어 생년월일이라는 정보를 토대로 BruteForce 무작위 대입을 시도해보려고 합니다

console을 이용하여 

```code
// 6자리 생년월일 정규 표현식
//생년월일  연 00~99 / 월 01-09 혹은 10-12 / 일 01-09 , 10-29, 30,31 /    
	const format = /([0-9]{2}(0[1-9]|1[0-2])(0[1-9]|[1,2][0-9]|3[0,1]))/;

// 어떤 생년월일을 입력했는지 console 로그를 찍기
function bircheck(birth){
	
	console.log("test :"+birth);
	if (format.test(birth)) _0x9a220(birth);	
	
}

// 생년월일 Bruteforce
for (let i = 600101; i<= 991231; i++){
	bircheck(i);
}



```

문제가 하나 발생했습니다.
반복문 하나할 때 마다 alert이 발생하여 확인버튼을 지속적으로 눌러줘야하는 문제 확인

<img width="3838" height="808" alt="image" src="https://github.com/user-attachments/assets/10d19208-3da3-45cc-bab7-1e870c1256d1" />




정확한 생년월일을 찾지 않고 한번만 입력되기만 해도 페이지 값이 변할 것으로 판단되기에 
alert 기능자체를 console 로그찍게끔 바꿨습니다.
```  code
window.alert = function(msg) {
	console.log("alert : " , msg);
 
};
```

최종 코드

``` code
window.alert = function(msg) {
	console.log("alert : " , msg);
 
};

	const format = /([0-9]{2}(0[1-9]|1[0-2])(0[1-9]|[1,2][0-9]|3[0,1]))/;


function bircheck(birth){
	
	console.log("test :"+birth);
	if (format.test(birth)) _0x9a220(birth);	
	
}


for (let i = 600101; i<= 991231; i++){
	bircheck(i);
}
```


<img width="2499" height="818" alt="image" src="https://github.com/user-attachments/assets/9cc095a5-7ae6-413d-9c44-0b6f2a87e1f1" />


