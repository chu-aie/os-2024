# `reverse` 프로젝트

**시작하기 전에:** [실습 튜토리얼](https://os2024.halla.ai/projects/lab-tutorial.html)을 읽어보세요. C 프로그래밍 환경에서 코딩할 때 유용한 팁들이 있습니다.

이 프로젝트는 전체 프로젝트가 어떻게 진행될지에 대해 익숙해지기 위한 간단한 준비 과제입니다. 또한 앞으로 몇 달 동안 매우 익숙해질 C 프로그래머의 사고방식에 빠져들게 하는 역할도 합니다. 행운을 빕니다!

여러분은 `reverse`라는 간단한 프로그램을 작성할 것입니다. 이 프로그램은 다음 중 한 가지 방법으로 실행되어야 합니다:

```sh
./reverse
./reverse input.txt
./reverse input.txt output.txt
```

위의 줄은 사용자가 역순 프로그램 `reverse`의 이름을 입력했다는 것을 의미합니다(`./`는 현재 작업 디렉토리(점이라고 하며 `.`로 참조됨)를 나타내고 슬래시(`/`)는 구분자입니다. 따라서 이 디렉토리에서 `reverse`라는 프로그램을 찾으세요). 그리고 명령줄 인수를 전혀 주지 않았거나, 하나의 명령줄 인수(입력 파일 `input.txt`), 또는 두 개의 명령줄 인수(입력 파일과 출력 파일 `output.txt`)를 주었습니다.

입력 파일은 다음과 같을 수 있습니다:

```
hello
this
is
a file
```

역순 프로그램의 목표는 지정된 입력 파일에서 데이터를 읽어 역순으로 만드는 것입니다. 따라서 입력 스트림의 역순으로 줄을 출력해야 합니다. 따라서 앞서 언급한 예제의 경우 출력은 다음과 같아야 합니다:

```
a file
is
this
hello
```

파일을 실행하는 다양한 방법(위에서 언급한 것처럼)은 이 간단한 새로운 유닉스 유틸리티를 사용하는 약간 다른 방법에 해당합니다. 예를 들어, 두 개의 명령줄 인수로 실행할 때 프로그램은 사용자가 제공한 입력 파일에서 읽어 들이고 사용자가 제공한 출력 파일에 해당 파일의 역순 버전을 작성해야 합니다.

하나의 명령줄 인수로만 실행되면 사용자는 입력 파일을 제공하지만 파일은 화면에 출력되어야 합니다. 유닉스 기반 시스템에서는 화면에 출력하는 것이 **표준 출력** 또는 줄여서 `stdout`이라고 하는 특수 파일에 쓰는 것과 같습니다.

마지막으로 인수 없이 실행되면 역순 프로그램은 사용자가 입력하는 **표준 입력**(`stdin`)에서 읽어 들이고 표준 출력(즉, 화면)에 씁니다.

쉽게 들리죠? 그래야 합니다. 하지만 몇 가지 세부사항이 있습니다...

## 세부사항

### 가정 및 오류

- **입력이 출력과 동일함:** 입력 파일과 출력 파일이 동일한 파일인 경우 "Input and output file must differ"라는 오류 메시지를 출력하고 반환 코드 1로 종료해야 합니다.

- **문자열 길이:** 한 줄이 얼마나 길어야 하는지에 대해 아무것도 가정해서는 안 됩니다. 따라서 매우 긴 입력 줄을 읽어야 할 수도 있습니다...

- **파일 길이:** 파일의 길이에 대해서도 아무것도 가정해서는 안 됩니다. 즉, 파일이 **매우** 길 수 있습니다.

- **잘못된 파일:** 사용자가 입력 파일이나 출력 파일을 지정했는데 어떤 이유로 해당 파일(예: `input.txt`)을 열려고 시도했지만 실패한 경우 다음과 같은 정확한 오류 메시지를 출력해야 합니다: `error: cannot open file 'input.txt'` 그런 다음 반환 코드 1로 종료합니다(즉, `exit(1);`을 호출합니다).

- **Malloc 실패:** `malloc()`을 호출하여 일부 메모리를 할당하는데 malloc이 실패하면 `malloc failed`라는 오류 메시지를 출력하고 반환 코드 1로 종료해야 합니다.

- **프로그램에 전달된 인수가 너무 많음:** 사용자가 `reverse`를 너무 많은 인수로 실행하면 `usage: reverse <input> <output>`를 출력하고 반환 코드 1로 종료합니다.

- **오류 메시지를 출력하는 방법:** 모든 오류에서 `fprintf()`를 사용하여 오류를 화면에 출력하고 오류 메시지를 `stdout`(표준 출력)이 아닌 `stderr`(표준 오류)로 보내야 합니다. 이는 C 코드에서 다음과 같이 수행됩니다: `fprintf(stderr, "whatever the error message is\n");`

## 유용한 루틴

종료하려면 `exit(1)`을 호출하세요. `exit()`에 전달하는 숫자(이 경우 1)는 프로그램이 오류를 반환했는지(즉, 0이 아닌 값을 반환했는지) 또는 깨끗하게 종료되었는지(즉, 0을 반환했는지) 확인하기 위해 사용자가 사용할 수 있습니다.

입력 파일을 읽어들이기 위해 다음 루틴을 사용하면 작업이 쉬워질 것입니다: `fopen()`, `getline()`, `fclose()`.

출력(화면 또는 파일)을 위해서는 `fprintf()`를 사용하세요. `stdout`을 `fprintf()`에 전달하면 표준 출력에 쉽게 쓸 수 있습니다. 또한 `fopen`에서 반환된 `FILE *`을 전달하면 파일에 쉽게 쓸 수 있습니다(예: `fp=fopen(...); fprintf(fp, ...);`).

`malloc()` 루틴은 메모리 할당에 유용합니다. 리스트에 요소를 추가하는 데 사용할 수 있겠네요?

이러한 함수들의 사용법을 모른다면 맨 페이지를 이용하세요. 예를 들어 명령줄에서 `man malloc`을 입력하면 malloc에 대한 많은 정보를 얻을 수 있습니다.

## 팁

**작게 시작하고 점진적으로 작동하게 만드세요.** 예를 들어 먼저 입력 파일을 한 번에 한 줄씩 읽어 들이고 읽은 내용을 출력하는 프로그램을 만드세요. 그런 다음 천천히 기능을 추가하고 진행하면서 테스트하세요.

예를 들어 우리가 이 코드를 작성한 방법은 먼저 `fopen()`, `getline()`, `fclose()`를 사용하여 입력 파일을 읽고 출력하는 코드를 작성하는 것이었습니다. 그런 다음 각 입력 줄을 연결 리스트에 저장하는 코드를 작성하고 제대로 작동하는지 확인했습니다. 그런 다음 리스트를 역순으로 출력했습니다. 그런 다음 오류 사례를 처리하는지 확인했습니다. 등등...

**테스팅이 중요합니다.** 우리가 알고 있던 훌륭한 프로그래머는 작성하는 코드 한 줄마다 5~10줄의 테스트 코드를 작성해야 한다고 말했습니다. 코드가 작동하는지 확인하기 위해 테스트하는 것이 중요합니다. 코드가 처리해야 한다고 생각하는 모든 경우를 처리하는지 확인하는 테스트를 작성하세요. 가능한 한 포괄적으로 하세요. 물론 우리가 여러분의 프로젝트를 채점할 때도 그럴 것입니다. 따라서 우리가 발견하기 전에 먼저 버그를 찾는 것이 좋습니다.

**이전 버전을 보관하세요.** 버그를 도입하고 쉽게 되돌릴 수 없을 수 있으므로 프로그램의 이전 버전 사본을 보관하세요. 이렇게 하는 간단한 방법은 개발 중 다양한 시점에서 파일의 사본을 명시적으로 만들어 사본을 보관하는 것입니다. 예를 들어 `reverse.c`의 간단한 버전(예: 파일을 읽기만 하는)이 작동한다고 가정해 봅시다. `cp reverse.c reverse.v1.c`를 입력하여 `reverse.v1.c` 파일에 사본을 만드세요. 보다 정교한 개발자는 git(github을 통해서 할 수도 있음)과 같은 버전 제어 시스템을 사용합니다. 이러한 도구는 배울 만한 가치가 충분히 있으므로 배우세요!

# 키 용어 설명

- **표준 입력(Standard Input)**: 프로그램이 데이터를 읽어들이는 기본 입력 스트림입니다. 일반적으로 키보드에서 사용자가 입력하는 데이터를 나타냅니다. `stdin`으로 참조됩니다.

- **표준 출력(Standard Output)**: 프로그램이 데이터를 출력하는 기본 출력 스트림입니다. 일반적으로 터미널 화면에 출력되는 데이터를 나타냅니다. `stdout`으로 참조됩니다.

- **표준 오류(Standard Error)**: 오류 메시지를 출력하기 위한 별도의 출력 스트림입니다. 이를 통해 일반 출력과 오류 메시지를 구분할 수 있습니다. `stderr`로 참조됩니다.

- **리디렉션(Redirection)**: 프로그램의 입력 또는 출력을 파일이나 다른 프로그램으로 재지정하는 것입니다. `<`는 입력 리디렉션에 사용되고 `>`는 출력 리디렉션에 사용됩니다.

- **연결 리스트(Linked List)**: 각 요소가 다음 요소에 대한 참조를 포함하는 선형 데이터 구조입니다. 동적 메모리 할당을 사용하여 구현되며 크기가 유연합니다.

- **역순(Reverse)**: 항목의 순서를 거꾸로 바꾸는 것입니다. 문자열이나 리스트와 같은 시퀀스에 적용할 수 있습니다.

# 코드 예제

입력 파일을 읽고 각 줄을 연결 리스트에 저장하는 방법의 예입니다:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_LINE_LENGTH 1024

typedef struct Node {
    char *data;
    struct Node *next;
} Node;

Node *head = NULL;

void append(char *line) {
    Node *newNode = (Node *)malloc(sizeof(Node));
    newNode->data = strdup(line);
    newNode->next = NULL;

    if (head == NULL) {
        head = newNode;
    } else {
        Node *curr = head;
        while (curr->next != NULL) {
            curr = curr->next;
        }
        curr->next = newNode;
    }
}

int main() {
    FILE *file = fopen("input.txt", "r");
    if (file == NULL) {
        fprintf(stderr, "error: cannot open file 'input.txt'\n");
        exit(1);
    }

    char line[MAX_LINE_LENGTH];
    while (fgets(line, sizeof(line), file)) {
        append(line);
    }

    fclose(file);

    // 여기서 연결 리스트를 처리할 수 있습니습니다
    return 0;
}
```

이 코드는 `fopen()`을 사용하여 "input.txt" 파일을 열고, `fgets()`를 사용하여 파일에서 한 줄씩 읽어들입니다. 각 줄은 `append()` 함수를 사용하여 연결 리스트에 추가됩니다. 연결 리스트의 노드는 `Node` 구조체로 표현되며, 각 노드는 줄의 데이터와 다음 노드에 대한 포인터를 포함합니다.

연결 리스트를 역순으로 출력하는 방법의 예입니다:

```c
void printReverse(Node *node) {
    if (node == NULL) {
        return;
    }
    printReverse(node->next);
    printf("%s", node->data);
}

// main 함수 내에서 호출
printReverse(head);
```

이 `printReverse()` 함수는 재귀를 사용하여 연결 리스트를 역순으로 순회합니다. 먼저 리스트의 끝까지 재귀 호출을 진행한 다음, 되돌아오면서 각 노드의 데이터를 출력합니다. 이렇게 하면 리스트가 역순으로 출력됩니다.

물론 이것은 간단한 예일 뿐이며, 실제 프로그램에서는 오류 처리, 메모리 해제, 명령줄 인수 처리 등 더 많은 작업이 필요할 것입니다. 하지만 이 예제는 문제를 해결하기 위한 기본적인 접근 방식을 보여줍니다.

# 결론

`reverse` 프로젝트는 C 프로그래밍과 시스템 프로그래밍의 기본 개념을 연습할 수 있는 좋은 기회입니다. 파일 I/O, 메모리 할당, 데이터 구조, 오류 처리 등을 다룹니다. 또한 문제를 작은 부분으로 나누고 점진적으로 해결하는 법을 배울 수 있습니다.

프로젝트에 도전할 때는 먼저 문제를 완전히 이해하는 것이 중요합니다. 입력이 어떻게 주어지고 출력이 어떻게 되어야 하는지 정확히 알아야 합니다. 그런 다음 문제를 더 작은 하위 문제로 나누고 한 번에 하나씩 해결하세요. 각 단계를 철저히 테스트하여 예상대로 작동하는지 확인하세요.

어려움에 부딪히면 친구들과 협력하고, 온라인 리소스를 활용하고, 필요하다면 도움을 요청하세요. 그러나 무엇보다 포기하지 마세요. 때로는 해결책을 찾는 데 시간이 걸릴 수 있지만, 끝까지 밀어붙이면 더 나은 프로그래머가 될 수 있습니다.

C는 매우 강력하지만 까다로운 언어라는 점을 기억하세요. 메모리 관리와 포인터 같은 저수준 개념을 다루려면 세심한 주의가 필요합니다. 그러나 이러한 개념을 마스터하면 시스템이 어떻게 작동하는지에 대한 깊은 이해를 얻을 수 있습니다.

마지막으로 프로그래밍을 즐기세요! 때로는 도전적이고 좌절스러울 수 있지만, 자신의 코드가 작동하는 것을 보는 것만큼 보람 있는 일도 없습니다. 이 프로젝트가 여러분의 프로그래밍 여정에 있어 값진 한 걸음이 되기를 바랍니다.

행운을 빕니다!