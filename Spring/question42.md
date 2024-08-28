#### MVC에 대해서 설명해주세요
- MVC 구조는 뭐 Controller -> Service -> Repository 이 구조로 개발하여 결합도를 낮추는
디자인 패턴을 말한다.

#### 새롭게 알게된 내용
```
    @PostMapping("/{store_id}/products")
    public ResponseDto<Void> createProduct(@PathVariable("store_id") UUID storeId,
                                           @RequestBody ProductCreateRequestDto requestDto) {
        productService.createProduct(storeId, requestDto);

        return ResponseDto.ok();
    }
```
- 이 코드를 예로 들면 해당 코드는 요청으로 전달받은 requestDto를 Service 계층에 그대로 전달한다. 
- 이 경우, Service에 필요한 데이터의 포맷이 Controller에 종속적이게 되고, MSA 구조로 바꿀 때 Service 계층을 모듈로 분리해야
하는데, 이때 해당 Dto Type을 사용할 수가 없다.
- 그리고! 트랜잭션으로 처리되어야 하는 DTO 항목이, 요청으로 들어온 값과 일치하지 않을 수도 있다.
![image](https://github.com/user-attachments/assets/223d6599-b12a-4e9e-8186-254c8d299f6e)

- 이렇게 사용자 요청의 파라미터를 통해 외부 API를 여러번 호출한 이후 Service 레이어를 호출하는 경우 or Service 계층을 호출하기 전에 다른 작업을 거치는 경우라면 Controller가
  받은 DTO와 Service가 받아야 할 DTO가 달라질 수 있다!

**그래서!!** Service가 원하는 포맷에 맞게 데이터를 전달하기 위해 Controller에서 해당 포맷 즉, ServiceDto로 변환하여 전달하는 방법이 있다!
- 이렇게 하면 각 계층이 담당하는 역할이 명확해지고 의존관계가 보다 명확해져 유지보수하기 좋은 형태가 된다!

  역시 골라 먹을 것이 많은 우아한형제기술블로그..
