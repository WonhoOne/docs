# Product Catalog v0.1.1

> Status: **Approved**. This catalog defines the shared offerings without fixing detailed Hotel, Transport, Meal option lists or prices.

## Theme offerings

| Theme | Included offering |
| --- | --- |
| `HONEYMOON_ROMANCE` | 2인 전용 로맨틱 스페셜 룸 데코레이션; 커플 기념 티셔츠; 2인 전용 고급차량 |
| `PARENTS_HEALING` | 고품격 안마/지압 서비스; 건강 인삼 기념품; 10인 승합 고급차량 |
| `GOLF_CHALLENGE` | 유명 골프 리조트 테마; 골프 액세서리 / 골프공; 10인 승합 고급차량 |
| `OUTDOOR_TREKKING` | 트레킹 / 산악 / 어드벤처 테마; 아웃도어 기념품 스카프; 10인 승합 고급차량 |

## Tour Style defaults

| TourStyle | Default composition |
| --- | --- |
| `CLASSIC` | 3성급 호텔; 도시락 식사 |
| `GRAND` | 4성급 호텔; 현지식 레스토랑 |
| `PREMIUM` | 5성급 호텔; 고급 레스토랑; 스테이크; 샴페인 |

추가 식음료 옵션에는 샴페인과 커피가 있습니다. 기타 추가 옵션은 상세 구현에서 확장할 수 있지만 승인된 계약을 깨지 않아야 합니다.

`HONEYMOON_ROMANCE`와 `PARENTS_HEALING`은 `GRAND` 또는 `PREMIUM`만 선택할 수 있습니다. 고객은 Tour Style을 선택한 뒤에도 Hotel, Transport, Meal을 개별 변경할 수 있습니다. Style의 기본 구성과 Theme 제공 항목의 조합/대체 방식 중 이 문서에 정하지 않은 세부사항은 임의로 확정하지 않습니다.
