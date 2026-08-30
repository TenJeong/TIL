# NAND flash와 FTL

## NAND 플래시 메모리

SSD의 저장 매체는 NAND 플래시 메모리이다. NAND 플래시 메모리의 구성 특성상, 특정 셀을 단독으로 읽고 쓰는 작업은 불가능하다. NAND 플래시의 기본 오퍼레이션의 특성은 다음과 같다.

- 페이지(Page): 읽기 및 쓰기의 단위
- 블록(Block): 페이지 수백 개의 묶음, 삭제 단위
- 읽기는 페이지 사이즈 단위
- 쓰기는 페이지 사이즈 단위
- 페이지는 덮어쓰기가 불가하다
- 삭제는 블록 사이즈 단위

## FTL

호스트에서는 SSD와 HDD는 모두 블록 단위의 인터페이스 사용을 기대한다. 하지만 LBA(Logical Block Address) array 등, "논리주소 X에 쓰고, 나중에 X를 덮어써라", 과 같은 구조는 HDD에 적합한 구조이다.
이에 NAND 플래시 메모리는 내부적인 특성을 숨기고 LBA array를 호스트로 노출하기 위해서 부가적인 컴포넌트를 필요로 한다.

이에 SSD 컨트롤러는 내부에서는 FTL(Flash Tranlation Layer)를 제공하며, 논리적 블록 맵핑(Logical block mapping)과 가비지 컬렉션(Garbage collection)을 수행한다.

- 논리적 블록 매핑(Logical block mapping):
호스트 영역의 논리주소(LBA, Logical Block Address)를 NAND 플래시 메모리의 물리적 주소(PBA, Physical Block Address)/물리적 페이지 주소(PPN)로 변환해주는 역할을 담당한다.

- 가비지 컬렉션(Garbage Collection):
무효화(stale) 페이지가 쌓이면, 그 블록의 유효 페이지만을 다른 블록 내 페이지 위치로 복사하고, 원본 블록을 지워서 다시 빈(free) 블록으로 회수한다.

## Write Amplification

덮어쓰기가 되지 않는 NAND 플래시 메모리 특성으로 인해, FTL은 out-of-place update를 수행한다. 이로써 원본 데이터가 있던 페이지는 무효(stale)상태가 된다. 빈(free) 상태로 만들어 공간을 되찾기 위해서는 GC가 유효 페이지를 다른 블록으로 복사한 뒤 블록을 지워야한다. 이 복사로 인해 SSD가 호스트 요청보다 물리적으로 더 많이 사용된다.

또한 쓰기 동작은 페이지 사이즈에 맞춰서 실행되므로, 페이지 사이즈에 일치하지 않는 모든 쓰기 동작은 필요 이상의 부가적인 쓰기(Write amplification)을 필요로 한다. 예를 들어 한 바이트 단위의 쓰기 동작은 하나의 페이지를 통째로 써야하므로, 페이지 크기와의 차이만큼의 추가적인 공간 또한 쓰기 동작에 포함된다.

따라서 WAF = 물리 쓰기 / 논리 쓰기가 된다.

## Wear leveling

NAND 플래시 메모리의 특성상, 프로그램 및 삭제 사이클(P/E Cycles) 횟수가 제한되어 있으며, 그에 따라 NAND 플래시 셀은 제한된 수명을 가지게 된다. 각 블록의 기대수명을 늘이기 위해, SSD 컨트롤러는 P/E Cycle이 고르게 분산된 쓰기를 실시한다.
