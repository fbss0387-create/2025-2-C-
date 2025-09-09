#include <stdio.h>

#define MAX_PRODUCTS 100

int main() {
    int numProducts;
    int stock[MAX_PRODUCTS + 1] = {0};    
    int incoming[MAX_PRODUCTS + 1] = {0}; 
    int sold[MAX_PRODUCTS + 1] = {0};     
    int id;
    int totalStock = 0;
    int totalSold = 0;
    int totalIncoming = 0;

    do {
        printf("상품 종류 개수를 입력하세요 (1~100): ");
        scanf("%d", &numProducts);
    } while (numProducts < 1 || numProducts > 100);

    printf("\n");

    for (int i = 1; i <= numProducts; i++) {
        printf("상품 ID %d의 입고 수량을 입력하세요: ", i);
        scanf("%d", &incoming[i]);
        stock[i] = incoming[i]; 
    }

    printf("\n");

    for (int i = 1; i <= numProducts; i++) {
        printf("상품 ID %d의 판매 수량을 입력하세요: ", i);
        scanf("%d", &sold[i]);

        if (sold[i] > stock[i]) {
            printf("경고: 상품 ID %d의 판매 수량이 재고보다 많습니다. 판매 수량을 재고만큼으로 조정합니다.\n", i);
            sold[i] = stock[i];
        }

        stock[i] -= sold[i]; 
    }

    printf("\n");

    printf("=== 모든 상품의 재고 수량 ===\n");
    for (int i = 1; i <= numProducts; i++) {
        printf("상품 ID %d: 재고 %d\n", i, stock[i]);
    }

    printf("\n");

    for (int i = 1; i <= numProducts; i++) {
        totalSold += sold[i];
        totalIncoming += incoming[i];
        totalStock += stock[i];
    }

    printf("전체 판매량: %d개\n", totalSold);

    float sellRate = 0.0;
    if (totalIncoming > 0) {
        sellRate = ((float)totalSold / totalIncoming) * 100.0;
    }
    printf("전체 판매율: %.2f%%\n", sellRate);

    int maxSold = sold[1], minSold = sold[1];
    int maxId = 1, minId = 1;

    for (int i = 2; i <= numProducts; i++) {
        if (sold[i] > maxSold) {
            maxSold = sold[i];
            maxId = i;
        }
        if (sold[i] < minSold) {
            minSold = sold[i];
            minId = i;
        }
    }

    printf("최대 판매량 상품: ID %d (판매량: %d)\n", maxId, maxSold);
    printf("최소 판매량 상품: ID %d (판매량: %d)\n", minId, minSold);

    printf("\n");

    printf("=== 재고 부족 경고 (재고 2 이하) ===\n");
    int warning = 0;
    for (int i = 1; i <= numProducts; i++) {
        if (stock[i] <= 2) {
            printf("상품 ID %d: 재고 부족! 현재 재고 = %d\n", i, stock[i]);
            warning = 1;
        }
    }
    if (!warning) {
        printf("모든 상품의 재고가 충분합니다.\n");
    }

    return 0;
}
