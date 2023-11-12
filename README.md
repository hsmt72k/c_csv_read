# c_csv_read

C言語でCSVファイルの1行をrecordとして、1項目をrecord.col[i].valueとして取得するためのコード例を以下に示します。この例では、各レコードは最大10個のカラムを持つと仮定しています。

``` c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_COLS 10
#define MAX_CHAR 100

// カラムを表す構造体
typedef struct {
    char value[MAX_CHAR];
} Column;

// レコードを表す構造体
typedef struct {
    Column col[MAX_COLS];
    int colCount;
} Record;

// CSVファイルから1行を読み込み、Record構造体に変換する関数
Record parseCsvLine(char* line) {
    Record record;
    record.colCount = 0;
    char* token = strtok(line, ",");

    while (token != NULL && record.colCount < MAX_COLS) {
        strncpy(record.col[record.colCount].value, token, MAX_CHAR);
        record.colCount++;
        token = strtok(NULL, ",");
    }

    return record;
}

// CSVファイルを読み込み、各行をRecord構造体として処理する関数
void processCsvFile(char* filename) {
    FILE* file = fopen(filename, "r");
    char buffer[MAX_CHAR];

    if (!file) {
        perror("File opening failed");
        return;
    }

    while (fgets(buffer, MAX_CHAR, file)) {
        // 改行文字を除去
        buffer[strcspn(buffer, "\n")] = 0;

        Record record = parseCsvLine(buffer);
        // レコードの処理
        // 例: record.col[0].value は最初のカラムの値
        printf("First Column: %s\n", record.col[0].value);
    }

    fclose(file);
}

int main() {
    char* filename = "example.csv";
    processCsvFile(filename);
    return 0;
}
```
