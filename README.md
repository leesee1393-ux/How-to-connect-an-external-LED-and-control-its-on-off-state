2026-07-14 
STM32F411을 기반으로 한 외부 LED GPIO 제어
=============
## 📋 프로젝트 개요

STM32F411을 사용하여 hal 모듈 없이 register를 직접 접근하여 외부 LED를 제어 및 구현 

### stm32F411의 메모리 맵핑 주소

![메모리 맵핑 주소]
<img width="826" height="683" alt="image" src="https://github.com/user-attachments/assets/a19dee71-c5b3-464e-957e-82d1829ef31a" />

![GPIO Port 모드 레지스터]

<img width="1070" height="412" alt="image" src="https://github.com/user-attachments/assets/b1da089a-e55a-4a44-822d-f3b1d5d24ff0" />

![GPIO port output 타입 레지스터]

<img width="1106" height="566" alt="image" src="https://github.com/user-attachments/assets/51329067-25f7-4331-ae9a-76ffb33bf5c0" />



### 구성 회로도
<img width="1422" height="1169" alt="image" src="https://github.com/user-attachments/assets/3ebc3132-5db2-410a-9a69-4a79cf52d447" />

### 구현 사진 
<img width="888" height="581" alt="image" src="https://github.com/user-attachments/assets/e8031d75-816e-4086-b073-6b0d6d99b22f" />


#### 주요 레지스터 주소

 - GPIO_MODER 0x40020000 : PortA 속성 설정 레지스터
 - GPIOA_OTYPER 0x40020004 : PortA출력 타입 설정 레지스터
 - GPIOA_ODR 0x40020014: PortA 출력 설정 레지스터


### stm32F411 메인 코드 
```c
#define GPIOA_MODER		(*(unsigned long *)0x40020000)  //STME32F411 PORTA 속성 설정 레지스터(PORT A 7번 핀 output 모드)
#define GPIOA_OTYPER	(*(unsigned long *)0x40020004)  // 출력 타입 설정 레지스터주소
#define GPIOA_ODR		(*(unsigned long *)0x40020014)   // PortA 출력 설정 레지스터

void Main(void)
{
	// LED GPA[7]를 출력(General open drain) 모드로 설정하시오 active low 

	GPIOA_MODER = 0x1 << 14; //(PORT A 7번 핀 output 모드)
	GPIOA_OTYPER = 0x1 << 7; // 출력 타입 설정 레지스터 (output 데이터 open drain 모드)

	// GPA[7] LED를 ON 시키도록 설정하시오

	GPIOA_ODR = 0x0 << 7;  // 출력 값(0)으로 설정 

}
```


#### 구현 사진

<img width="1848" height="1778" alt="image" src="https://github.com/user-attachments/assets/c9c35977-3a0f-4c67-aff6-fe79047d5798" />








## 보안점

### 1. datasheet 읽고 코드 구현하는게 익숙하지 않음 

    datasheet보고 stm32 코드 작성이 익숙하지 않아 구현이 어려움
    
