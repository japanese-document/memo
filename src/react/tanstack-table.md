{ "category": "React",  "order": 3, "date": "2024-03-26 23:10" }
---
# TanStack Tableメモ

## [useReactTable](https://tanstack.com/table/latest/docs/api/core/table#usereacttable--createsolidtable--useqwiktable--usevuetable--createsveltetable)

### data
TDataの配列

### columns
columnHelper.accessor等を使って作成する


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

* https://github.com/TanStack/table/blob/main/examples/react/basic/src/main.tsx

* https://tanstack.com/table/latest/docs/framework/react/examples/basic  

### aggregatedCell

Accessor columnsのaggregatedCellの例

* https://github.com/TanStack/table/blob/main/examples/react/grouping/src/main.tsx

* https://tanstack.com/table/latest/docs/framework/react/examples/grouping

## State

`on[State]Change`は`onColumnFiltersChange`や`onSortingChange`等の総称です。`[State]`には`state`のキーが入ります。

`on[State]Change`や`onStateChange`の引数には値かReactの`setState()`コールバック関数が渡される。  
https://tanstack.com/table/latest/docs/framework/react/guide/table-state#on-state-change-callbacks

State用の[型](https://tanstack.com/table/latest/docs/framework/react/guide/table-state#state-types)がある。
