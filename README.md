# ADSAPRAC


/* 1. MERGE SORT */

#include <stdio.h>

void merge(int arr[], int left, int mid, int right) {
    int temp[1000];
    int i = left, j = mid + 1, k = 0;

    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j])
            temp[k++] = arr[i++];
        else
            temp[k++] = arr[j++];
    }

    while (i <= mid)
        temp[k++] = arr[i++];

    while (j <= right)
        temp[k++] = arr[j++];

    for (i = 0; i < k; i++)
        arr[left + i] = temp[i];
}

void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

int main() {
    int arr[1000], n, i;

    printf("Enter number of elements: ");
    scanf("%d", &n);

    if (n < 1 || n > 1000)
        return 0;

    printf("Enter elements: ");
    for (i = 0; i < n; i++)
        scanf("%d", &arr[i]);

    mergeSort(arr, 0, n - 1);

    printf("Sorted array: ");
    for (i = 0; i < n; i++)
        printf("%d ", arr[i]);

    return 0;
}


/* 2. QUICK SORT */

#include <stdio.h>

void swap(int *first, int *second) {
    int temp = *first;
    *first = *second;
    *second = temp;
}

int partition(int arr[], int low, int high) {
    int pivot = arr[high];
    int i = low - 1, j;

    for (j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(&arr[i], &arr[j]);
        }
    }

    swap(&arr[i + 1], &arr[high]);
    return i + 1;
}

void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pivotIndex = partition(arr, low, high);
        quickSort(arr, low, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, high);
    }
}

int main() {
    int arr[1000], n, i;

    printf("Enter number of elements: ");
    scanf("%d", &n);

    if (n < 1 || n > 1000)
        return 0;

    printf("Enter elements: ");
    for (i = 0; i < n; i++)
        scanf("%d", &arr[i]);

    quickSort(arr, 0, n - 1);

    printf("Sorted array: ");
    for (i = 0; i < n; i++)
        printf("%d ", arr[i]);

    return 0;
}


/* 3. BINARY SEARCH TREE: INSERT, DELETE, SEARCH, DISPLAY */

#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *left;
    struct Node *right;
};

struct Node *createNode(int value) {
    struct Node *newNode = malloc(sizeof(struct Node));

    if (newNode == NULL)
        exit(1);

    newNode->data = value;
    newNode->left = NULL;
    newNode->right = NULL;

    return newNode;
}

struct Node *insert(struct Node *root, int value) {
    if (root == NULL)
        return createNode(value);

    if (value < root->data)
        root->left = insert(root->left, value);
    else if (value > root->data)
        root->right = insert(root->right, value);

    return root;
}

struct Node *search(struct Node *root, int value) {
    if (root == NULL || root->data == value)
        return root;

    if (value < root->data)
        return search(root->left, value);

    return search(root->right, value);
}

struct Node *findMin(struct Node *root) {
    while (root != NULL && root->left != NULL)
        root = root->left;

    return root;
}

struct Node *deleteNode(struct Node *root, int value) {
    struct Node *temp;

    if (root == NULL)
        return NULL;

    if (value < root->data) {
        root->left = deleteNode(root->left, value);
    } else if (value > root->data) {
        root->right = deleteNode(root->right, value);
    } else {
        if (root->left == NULL) {
            temp = root->right;
            free(root);
            return temp;
        }

        if (root->right == NULL) {
            temp = root->left;
            free(root);
            return temp;
        }

        temp = findMin(root->right);
        root->data = temp->data;
        root->right = deleteNode(root->right, temp->data);
    }

    return root;
}

void inorder(struct Node *root) {
    if (root != NULL) {
        inorder(root->left);
        printf("%d ", root->data);
        inorder(root->right);
    }
}

void freeTree(struct Node *root) {
    if (root != NULL) {
        freeTree(root->left);
        freeTree(root->right);
        free(root);
    }
}

int main() {
    struct Node *root = NULL;
    int choice, value;

    while (1) {
        printf("\n1. Insert\n2. Delete\n3. Search\n4. Display\n5. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter value: ");
                scanf("%d", &value);
                root = insert(root, value);
                break;

            case 2:
                printf("Enter value to delete: ");
                scanf("%d", &value);
                root = deleteNode(root, value);
                break;

            case 3:
                printf("Enter value to search: ");
                scanf("%d", &value);

                if (search(root, value) != NULL)
                    printf("Value found\n");
                else
                    printf("Value not found\n");
                break;

            case 4:
                printf("Inorder traversal: ");
                inorder(root);
                printf("\n");
                break;

            case 5:
                freeTree(root);
                return 0;

            default:
                printf("Invalid choice\n");
        }
    }
}


/* 4. 0/1 KNAPSACK USING DYNAMIC PROGRAMMING */

#include <stdio.h>

int max(int first, int second) {
    return first > second ? first : second;
}

int main() {
    int weight[101], value[101];
    int dp[101][1001];
    int numberOfItems, capacity;
    int i, currentCapacity;

    printf("Enter number of items: ");
    scanf("%d", &numberOfItems);

    if (numberOfItems < 1 || numberOfItems > 100)
        return 0;

    printf("Enter weights: ");
    for (i = 1; i <= numberOfItems; i++)
        scanf("%d", &weight[i]);

    printf("Enter values: ");
    for (i = 1; i <= numberOfItems; i++)
        scanf("%d", &value[i]);

    printf("Enter knapsack capacity: ");
    scanf("%d", &capacity);

    if (capacity < 0 || capacity > 1000)
        return 0;

    for (i = 0; i <= numberOfItems; i++) {
        for (currentCapacity = 0;
             currentCapacity <= capacity;
             currentCapacity++) {

            if (i == 0 || currentCapacity == 0) {
                dp[i][currentCapacity] = 0;
            } else if (weight[i] <= currentCapacity) {
                dp[i][currentCapacity] = max(
                    value[i] + dp[i - 1][currentCapacity - weight[i]],
                    dp[i - 1][currentCapacity]
                );
            } else {
                dp[i][currentCapacity] =
                    dp[i - 1][currentCapacity];
            }
        }
    }

    printf("Maximum value: %d\n", dp[numberOfItems][capacity]);

    return 0;
}


/* 5. N-QUEENS USING BACKTRACKING */

#include <stdio.h>

int queenColumn[20];
int boardSize;
int solutionCount = 0;

int isSafe(int row, int column) {
    int previousRow;

    for (previousRow = 1; previousRow < row; previousRow++) {
        if (queenColumn[previousRow] == column)
            return 0;

        if (queenColumn[previousRow] - column ==
            previousRow - row)
            return 0;

        if (column - queenColumn[previousRow] ==
            previousRow - row)
            return 0;
    }

    return 1;
}

void solveNQueens(int row) {
    int column, i;

    if (row > boardSize) {
        solutionCount++;

        printf("Solution %d: ", solutionCount);
        for (i = 1; i <= boardSize; i++)
            printf("%d ", queenColumn[i]);

        printf("\n");
        return;
    }

    for (column = 1; column <= boardSize; column++) {
        if (isSafe(row, column)) {
            queenColumn[row] = column;
            solveNQueens(row + 1);
        }
    }
}

int main() {
    printf("Enter board size (1-19): ");
    scanf("%d", &boardSize);

    if (boardSize < 1 || boardSize > 19)
        return 0;

    solveNQueens(1);

    if (solutionCount == 0)
        printf("No solution exists\n");

    return 0;
}


/* 6. JOB SEQUENCING WITH DEADLINES */

#include <stdio.h>

struct Job {
    int id;
    int deadline;
    int profit;
};

void sortJobs(struct Job jobs[], int numberOfJobs) {
    int i, j;
    struct Job temp;

    for (i = 0; i < numberOfJobs - 1; i++) {
        for (j = 0; j < numberOfJobs - i - 1; j++) {
            if (jobs[j].profit < jobs[j + 1].profit) {
                temp = jobs[j];
                jobs[j] = jobs[j + 1];
                jobs[j + 1] = temp;
            }
        }
    }
}

int main() {
    struct Job jobs[100];
    int slots[101] = {0};
    int numberOfJobs, i, j;
    int maximumDeadline = 0;
    int totalProfit = 0, jobsScheduled = 0;

    printf("Enter number of jobs: ");
    scanf("%d", &numberOfJobs);

    if (numberOfJobs < 1 || numberOfJobs > 100)
        return 0;

    for (i = 0; i < numberOfJobs; i++) {
        jobs[i].id = i + 1;

        printf("Enter deadline and profit for job %d: ", i + 1);
        scanf("%d %d", &jobs[i].deadline, &jobs[i].profit);

        if (jobs[i].deadline < 0)
            return 0;

        if (jobs[i].deadline > maximumDeadline)
            maximumDeadline = jobs[i].deadline;
    }

    sortJobs(jobs, numberOfJobs);

    if (maximumDeadline > 100)
        maximumDeadline = 100;

    for (i = 0; i < numberOfJobs; i++) {
        for (j = jobs[i].deadline; j >= 1; j--) {
            if (j <= maximumDeadline && slots[j] == 0) {
                slots[j] = jobs[i].id;
                totalProfit += jobs[i].profit;
                jobsScheduled++;
                break;
            }
        }
    }

    printf("Scheduled jobs: ");
    for (i = 1; i <= maximumDeadline; i++) {
        if (slots[i] != 0)
            printf("J%d ", slots[i]);
    }

    printf("\nNumber of jobs scheduled: %d", jobsScheduled);
    printf("\nMaximum profit: %d\n", totalProfit);

    return 0;
}


/* 7. SUM OF SUBSETS USING BACKTRACKING */

#include <stdio.h>

int elements[100];
int numberOfElements;
int targetSum;
int subset[100];
int solutionCount = 0;

void sumOfSubsets(int index, int currentSum) {
    int i;

    if (currentSum == targetSum) {
        solutionCount++;

        printf("Subset %d: { ", solutionCount);
        for (i = 0; i < index; i++) {
            if (subset[i] == 1)
                printf("%d ", elements[i]);
        }
        printf("}\n");
        return;
    }

    if (index == numberOfElements || currentSum > targetSum)
        return;

    subset[index] = 1;
    sumOfSubsets(index + 1, currentSum + elements[index]);

    subset[index] = 0;
    sumOfSubsets(index + 1, currentSum);
}

int main() {
    int i;

    printf("Enter number of elements: ");
    scanf("%d", &numberOfElements);

    if (numberOfElements < 1 || numberOfElements > 100)
        return 0;

    printf("Enter positive elements: ");
    for (i = 0; i < numberOfElements; i++) {
        scanf("%d", &elements[i]);

        if (elements[i] <= 0)
            return 0;
    }

    printf("Enter target sum: ");
    scanf("%d", &targetSum);

    if (targetSum < 0)
        return 0;

    sumOfSubsets(0, 0);

    if (solutionCount == 0)
        printf("No subset found\n");

    return 0;
}


/* 8. BELLMAN-FORD ALGORITHM */

#include <stdio.h>
#include <limits.h>

struct Edge {
    int source;
    int destination;
    int weight;
};

int main() {
    struct Edge edges[1000];
    int distance[100];
    int vertices, numberOfEdges, sourceVertex;
    int i, j, u, v, weight;

    printf("Enter number of vertices and edges: ");
    scanf("%d %d", &vertices, &numberOfEdges);

    if (vertices < 1 || vertices > 100 ||
        numberOfEdges < 0 || numberOfEdges > 1000)
        return 0;

    printf("Enter source, destination and weight for each edge:\n");

    for (i = 0; i < numberOfEdges; i++) {
        scanf("%d %d %d",
              &edges[i].source,
              &edges[i].destination,
              &edges[i].weight);

        if (edges[i].source < 0 || edges[i].source >= vertices ||
            edges[i].destination < 0 ||
            edges[i].destination >= vertices)
            return 0;
    }

    printf("Enter source vertex (0 to %d): ", vertices - 1);
    scanf("%d", &sourceVertex);

    if (sourceVertex < 0 || sourceVertex >= vertices)
        return 0;

    for (i = 0; i < vertices; i++)
        distance[i] = INT_MAX;

    distance[sourceVertex] = 0;

    for (i = 1; i < vertices; i++) {
        for (j = 0; j < numberOfEdges; j++) {
            u = edges[j].source;
            v = edges[j].destination;
            weight = edges[j].weight;

            if (distance[u] != INT_MAX &&
                (long long)distance[u] + weight < distance[v]) {
                distance[v] = distance[u] + weight;
            }
        }
    }

    for (j = 0; j < numberOfEdges; j++) {
        u = edges[j].source;
        v = edges[j].destination;
        weight = edges[j].weight;

        if (distance[u] != INT_MAX &&
            (long long)distance[u] + weight < distance[v]) {
            printf("Negative weight cycle detected\n");
            return 0;
        }
    }

    printf("Vertex\tShortest Distance\n");

    for (i = 0; i < vertices; i++) {
        if (distance[i] == INT_MAX)
            printf("%d\tUnreachable\n", i);
        else
            printf("%d\t%d\n", i, distance[i]);
    }

    return 0;
}


/* 9. TRAVELLING SALESMAN PROBLEM USING DP AND BITMASKING */

#include <stdio.h>

#define MAX_CITIES 15
#define INF 1000000000

int cost[MAX_CITIES][MAX_CITIES];
int dp[1 << MAX_CITIES][MAX_CITIES];
int numberOfCities;

int min(int first, int second) {
    return first < second ? first : second;
}

int tsp(int visitedMask, int currentCity) {
    int nextCity;
    int answer = INF;
    int allVisited = (1 << numberOfCities) - 1;
    int remainingCost;

    if (visitedMask == allVisited) {
        if (cost[currentCity][0] == -1)
            return INF;

        return cost[currentCity][0];
    }

    if (dp[visitedMask][currentCity] != -1)
        return dp[visitedMask][currentCity];

    for (nextCity = 0; nextCity < numberOfCities; nextCity++) {
        if (!(visitedMask & (1 << nextCity)) &&
            cost[currentCity][nextCity] != -1) {

            remainingCost = tsp(
                visitedMask | (1 << nextCity),
                nextCity
            );

            if (remainingCost != INF) {
                answer = min(
                    answer,
                    cost[currentCity][nextCity] + remainingCost
                );
            }
        }
    }

    dp[visitedMask][currentCity] = answer;
    return answer;
}

int main() {
    int i, j, minimumCost;

    printf("Enter number of cities (2-15): ");
    scanf("%d", &numberOfCities);

    if (numberOfCities < 2 || numberOfCities > MAX_CITIES)
        return 0;

    printf("Enter cost matrix (-1 for no edge):\n");

    for (i = 0; i < numberOfCities; i++) {
        for (j = 0; j < numberOfCities; j++)
            scanf("%d", &cost[i][j]);
    }

    for (i = 0; i < (1 << numberOfCities); i++) {
        for (j = 0; j < numberOfCities; j++)
            dp[i][j] = -1;
    }

    minimumCost = tsp(1, 0);

    if (minimumCost == INF)
        printf("No complete tour exists\n");
    else
        printf("Minimum tour cost: %d\n", minimumCost);

    return 0;
}


/* 10. NAIVE STRING MATCHING */

#include <stdio.h>
#include <string.h>

int main() {
    char text[1000], pattern[1000];
    int textLength, patternLength;
    int i, j, found = 0;

    printf("Enter text (without spaces): ");
    scanf("%999s", text);

    printf("Enter pattern (without spaces): ");
    scanf("%999s", pattern);

    textLength = strlen(text);
    patternLength = strlen(pattern);

    if (patternLength > textLength) {
        printf("Pattern not found\n");
        return 0;
    }

    for (i = 0; i <= textLength - patternLength; i++) {
        for (j = 0; j < patternLength; j++) {
            if (text[i + j] != pattern[j])
                break;
        }

        if (j == patternLength) {
            printf("Pattern found at index %d\n", i);
            found = 1;
        }
    }

    if (!found)
        printf("Pattern not found\n");

    return 0;
}
