# Operating Systems Homework

這個 repository 整理我在作業系統課程中完成的四次作業，內容從 NachOS 的 System Call、資料排序與 Process / Thread，到 CPU Scheduling 和 Memory Management。

我把每次作業的重點、主要實作內容和我實際練習到的觀念整理在下面，方便之後複習，也可以快速看出每份作業在做什麼。

---

## 作業總覽

| Homework | 主題 | 主要內容 |
|---|---|---|
| HW1 | NachOS System Call | `Open()`、`Write()`、`Read()`、`Close()` |
| HW2 | Million Numbers Sorting | Bubble Sort、Merge Sort、Process、Thread |
| HW3 | NachOS CPU Scheduling | Priority、SJF、FCFS |
| HW4 | NachOS Memory Management | Page Table、Frame Table、MemoryLimitException |

---

## HW1 - NachOS System Call

### 作業內容

這次作業主要是在 NachOS 中完成基本的檔案 I/O System Call：

- `Open()`
- `Write()`
- `Read()`
- `Close()`

並透過兩個測試程式確認功能是否可以正常執行。

### 測試內容

`fileIO_test1`

- Open
- Write
- Close

`fileIO_test2`

- Open
- Read
- Close

### 我這次主要練習到

- System Call 的基本流程
- NachOS 中 User Program 與 Kernel 的互動
- 檔案開啟、讀取、寫入與關閉
- 如何透過測試程式確認 System Call 是否正確

---

## HW2 - Million Numbers Sorting

### 作業內容

這次作業是針對大量整數資料進行排序，並比較不同方法在不同資料量和不同 K 值下的執行時間。

測試資料包含：

- 10,000 筆
- 100,000 筆
- 500,000 筆
- 1,000,000 筆

程式需要把四種方法整合在同一支程式中，並依照輸入的檔名、K 值和方法執行。

### 四種方法

#### Method 1

直接對全部資料進行 Bubble Sort。

```text
Input
  ↓
Bubble Sort
  ↓
Output
```

#### Method 2

先把 N 筆資料切成 K 份，由一個 process 對每一份資料做 Bubble Sort，最後再進行 Merge Sort。

```text
Input
  ↓
Split into K parts
  ↓
Bubble Sort
  ↓
Merge Sort
  ↓
Output
```

#### Method 3

把資料切成 K 份，建立 K 個 processes 分別進行 Bubble Sort，再由另外的 process(es) 進行 Merge Sort。

#### Method 4

把資料切成 K 份，建立 K 個 threads 分別進行 Bubble Sort，再由 K-1 個 thread(s) 進行 Merge Sort。

### 輸出內容

排序完成後，除了排序結果之外，也需要記錄：

- CPU Time
- Output Time

### 我這次主要練習到

- Bubble Sort
- Merge Sort
- Process
- Thread
- 將資料切成多份處理
- 比較不同 N、K 和方法的執行時間
- 觀察 Process / Thread 對執行效率的影響

---

## HW3 - NachOS CPU Scheduling

### 作業內容

這次作業是在 NachOS 中完成三種 CPU Scheduling：

- Priority
- SJF
- FCFS

並透過 `-sche` 參數指定要執行的排程方法。

### 執行方式

Priority：

```bash
./build.linux/nachos -sche Priority
```

SJF：

```bash
./build.linux/nachos -sche SJF
```

FCFS：

```bash
./build.linux/nachos -sche FCFS
```

### 三種排程

#### Priority

根據 Priority 決定執行順序。

#### SJF

依照工作長度決定執行順序，較短的工作先執行。

#### FCFS

依照進入 Ready Queue 的先後順序執行。

### 我這次主要練習到

- CPU Scheduling
- Priority Scheduling
- Shortest Job First
- First Come First Served
- NachOS 中排程方法的修改與測試
- 不同 Scheduling Policy 的執行差異

---

## HW4 - NachOS Memory Management

> LAB3 為 HW4 的作業說明。

### 作業內容

這次作業主要是在 NachOS 中實作 Memory Management，目標是處理 Page Table 實作不完整造成的問題。

主要需要處理：

- `MemoryLimitException`
- Frame Table
- Page Table 初始化
- Physical Page 配置
- Virtual Page 與 Physical Page 的對應

### MemoryLimitException

當 Virtual Page 的需求超過 Physical Page 可以提供的範圍時，需要產生 `MemoryLimitException`。

### Frame Table

在 `threads/kernel.*` 中建立並實作 Frame Table，用來記錄 Physical Page 的使用狀態。

### Page Table

在：

```cpp
AddrSpace::AddrSpace()
```

中初始化 Page Table。

在：

```cpp
AddrSpace::Load(char *fileName)
```

中需要：

- 檢查 Virtual Page index 是否超過 Physical Page index
- 超過時產生 `MemoryLimitException`
- 找到可以使用的 Physical address
- 在 Page Table 中記錄相關資訊

### 我這次主要練習到

- Virtual Page
- Physical Page
- Page Table
- Frame Table
- Physical Memory 配置
- Memory Limit 的例外處理
- NachOS 的 Memory Management 流程

---

## Repository Structure

```text
NachOS_hw
│
├── nachos_hw1.gz
│   └── HW1 - NachOS System Call
│
├── nachos_hw2/
│   └── HW2 - Million Numbers Sorting
│
├── nachos_hw3.gz
│   └── HW3 - NachOS CPU Scheduling
│
├── nachos_hw4.gz
│   └── HW4 - NachOS Memory Management
│
└── README.md
```

---

## 總結

這四次作業的內容是一路從：

```text
System Call
    ↓
Process / Thread
    ↓
CPU Scheduling
    ↓
Memory Management
```

慢慢把作業系統裡幾個重要的主題實際做過一次。

對我來說，這些作業不只是把功能寫出來而已，還需要自己看懂 NachOS 原本的架構、找到要修改的位置，再用測試結果確認自己的實作有沒有正確。

也因為真的有去改程式和跑測試，所以比只看課本更容易理解 System Call、Scheduling、Process / Thread 和 Memory Management 之間的關係。
