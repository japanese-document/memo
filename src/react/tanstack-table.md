{ "category": "React",  "order": 3, "date": "2024-03-24 13:10" }
---
# TanStack Tableメモ

## TanStack TableのTData

TanStack Tableの`TData`はデーブルの行を表す[オブジェクトの型](https://tanstack.com/table/latest/docs/guide/tables#defining-data)です。

## Accessor Columns

### accessor key

`columnHelper.accessor()`の第1引数または`accessorKey`には`TData`の[キーを指定します](https://tanstack.com/table/latest/docs/guide/column-defs#object-keys)。

### 動的に値を生成する

`columnHelper.accessor()`の第1引数もしくは`accessorFn`に[accessor functions](https://tanstack.com/table/latest/docs/guide/column-defs#accessor-functions)を渡します。

### id

accessor keyは[idにになる](https://tanstack.com/table/latest/docs/guide/column-defs#unique-column-ids)。

### footerとheader

Accessor columnsのfooterとheaderの例

https://github.com/TanStack/table/blob/main/examples/react/basic/src/main.tsx
https://tanstack.com/table/latest/docs/framework/react/examples/basic  
