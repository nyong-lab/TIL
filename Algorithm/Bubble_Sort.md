# Bubble Sort
> 오름차순을 기준으로 정렬  

- 서로 인접한 두 원소를 검사하여 정렬하는 알고리즘  
- 선택정렬과 기본 개념이 유사하다  

버블정렬은 첫 번째 자료와 두 번째 자료를, 두 번째 자료와 세 번째 자료를, 세 번째와 네 번째를,  
이런식으로 (마지막-1)번째 자료와 마지막 자료를 비교하여 교환하면서 자료를 정렬한다.  
1회전을 수행하고 나면 가장 큰 자료가 맨 뒤로 이동하므로 2회전에서는 맨 끝에 있는 자료는 정렬에서 제외되고,  
2회전을 수행하고 나면 끝에서 두 번째 자료까지는 정렬에서 제외된다.  
이렇게 정렬을 1회전 수행할 때마다 정렬에서 제외되는 데이터가 하나씩 늘어난다.  

```java
static public void bubbleSort(int n, String [] names, int [] numbers){
	for(int i=n-1; i>0; i--){ //0~(i-1)까지 반복
		for(int j=0; j<i; j++){ //j번째와 j+1번째의 요소가 크기 순이 아니면 교환
			if(numbers[j] > numbers[j+1]){
				//swap numbers[j] and numbers[j+1]
				String tmpstr = numbers[j];
				numbers[j] = numbers[j+1];
				numbers[j+1] = tmpstr;

				String tmpstr = names[j]; //이름도 함께 바꿔줌
				names[j] = names[j+1];
				names[j+1] = tmpstr;
				}
			}
		}
}
```
