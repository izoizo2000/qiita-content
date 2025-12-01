---
title: 【C#で学ぶ】応用情報技術者試験のためのアルゴリズム入門：マージソート編
tags:
  - C#
  - アルゴリズム
  - 応用情報技術者試験
  - マージソート
private: false
updated_at: '2025-04-10T09:27:59+09:00'
id: e63adbbab74ae25e670f
organization_url_name: null
slide: false
ignorePublish: false
---
こんにちは！応用情報技術者試験（AP）の受験者向けに、これまで「探索」や「ソート」のアルゴリズムを解説してきました。今回は「マージソート」を取り上げます。分割統治法を活用した効率的なソートアルゴリズムで、試験でも理解しておくと役立つトピックです。C#での実装とともに学びましょう！

# マージソートとは？
マージソートは、配列を小さな部分に分割し、それぞれをソートしてから統合（マージ）することで全体を整列するアルゴリズムです。「分割統治法」の代表例であり、再帰的な処理が特徴です。

### マージソートの流れ
1. 配列を半分に分割する（これを再帰的に繰り返す）
2. 分割された小さな配列をソートしながら統合する
3. 最終的に全体がソートされた状態になる

### 特徴
* **時間計算量**: O(n log n)（nは要素数）
* **メリット**: 安定した性能（最悪でもO(n log n)）、安定ソート（同値要素の順序が保たれる）
* **デメリット**: 追加のメモリが必要（インプレースではない）

### C#での実装例
以下は、配列を昇順に整列するマージソートのコードです。再帰とマージ処理を分けて実装します。

```csharp
using System;

class Program
{
    static void MergeSort(int[] array, int left, int right)
    {
        if (left < right)
        {
            int mid = (left + right) / 2;
            MergeSort(array, left, mid); // 左半分をソート
            MergeSort(array, mid + 1, right); // 右半分をソート
            Merge(array, left, mid, right); // 統合
        }
    }

    static void Merge(int[] array, int left, int mid, int right)
    {
        int n1 = mid - left + 1; // 左半分のサイズ
        int n2 = right - mid; // 右半分のサイズ

        // 一時配列を作成
        int[] leftArray = new int[n1];
        int[] rightArray = new int[n2];

        // データを一時配列にコピー
        for (int i = 0; i < n1; i++)
            leftArray[i] = array[left + i];
        for (int j = 0; j < n2; j++)
            rightArray[j] = array[mid + 1 + j];

        // マージ処理
        int leftIndex = 0, rightIndex = 0, mergedIndex = left;
        while (leftIndex < n1 && rightIndex < n2)
        {
            if (leftArray[leftIndex] <= rightArray[rightIndex])
            {
                array[mergedIndex] = leftArray[leftIndex];
                leftIndex++;
            }
            else
            {
                array[mergedIndex] = rightArray[rightIndex];
                rightIndex++;
            }
            mergedIndex++;
        }

        // 残りの要素をコピー
        while (leftIndex < n1)
        {
            array[mergedIndex] = leftArray[leftIndex];
            leftIndex++;
            mergedIndex++;
        }
        while (rightIndex < n2)
        {
            array[mergedIndex] = rightArray[rightIndex];
            rightIndex++;
            mergedIndex++;
        }
    }

    static void Main()
    {
        int[] numbers = { 5, 3, 8, 4, 2 };
        Console.WriteLine("ソート前: " + string.Join(", ", numbers));

        MergeSort(numbers, 0, numbers.Length - 1);
        Console.WriteLine("ソート後: " + string.Join(", ", numbers));
    }
}
```

### 実行結果
```text
ソート前: 5, 3, 8, 4, 2
ソート後: 2, 3, 4, 5, 8
```


# マージソートの仕組みをステップで解説
### 初期配列: [5, 3, 8, 4, 2]
1. **分割**
[5, 3, 8, 4, 2] → [5, 3] と [8, 4, 2]
さらに [5, 3] → [5] と [3]、[8, 4, 2] → [8] と [4, 2]
2. **マージ**
[5] と [3] を統合 → [3, 5]
[4, 2] をソートして統合 → [2, 4]
[8] と [2, 4] を統合 → [2, 4, 8]
最後に [3, 5] と [2, 4, 8] を統合 → [2, 3, 4, 5, 8]

分割と統合を繰り返すことで、効率的にソートが完了します。

# 試験対策のポイント
1. **分割統治法を理解する**
マージソートは「分割」「処理」「統合」の3ステップが基本。試験で再帰処理を追う問題が出たら、この流れを思い出してください。

2. **時間計算量を押さえる**
分割がlog n回、マージが各段階でnの処理なのでO(n log n)。ヒープソートやクイックソートと比較して安定性が特徴です。

3. **メモリ使用量に注意**
マージソートは一時配列が必要なので、メモリ制約がある問題では不利になる可能性を覚えておきましょう。

# おわりに
マージソートは、分割統治法を活用した洗練されたアルゴリズムです。応用情報技術者試験では、アルゴリズムの効率性や特徴を問う問題が出やすいので、マージソートの安定性やO(n log n)の性能をしっかり理解しておくと有利です。試験まであと少し、頑張りましょう！
