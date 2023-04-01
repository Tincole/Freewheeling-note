[参考](https://github.com/pmndrs/zustand)
状态共享、状态变更、状态派生
## 默认不需要Provider

```js
// store.ts
import { create } from 'zustand'

const useBearStore = create((set) => ({
  bears: 0,
  increasePopulation: () => set((state) => ({ bears: state.bears + 1 })),
  removeAllBears: () => set({ bears: 0 }),
}))

// bears.tsx
import { useStore } from './store';

function Counter() {
  const { bears } = useStore()
  return <h1>{bears}</h1>  
}
```

## 不区分异步和同步
```js
// zustand store 写法

// store.ts
import create from 'zustand';

const initialState = {
 // ...
};

export const useStore = create((set, get) => ({
  ...initialState,
  createNewDesignSystem: async () => {
    const { params, toggleLoading } = get();

    toggleLoading();
    const res = await dispatch('/hitu/remote/create-new-ds', params);
    toggleLoading();

    if (!res) return;

    set({ created: true, designId: res.id });
  },
  toggleLoading: () => {
    set({ loading: !get().loading });
  }
}));

// CreateForm.tsx
import { useStore } from './store';

const CreateForm: FC = () => {
  const { createNewDesignSystem } = useStore();

  // ...
}
```
##  默认将所有的函数保持同一引用
不会造成重复渲染

## **方法需要调用当前快照下的值或方法**
通过get方法拿到最新的值，mobx是通过this拿到，context方式需要进行依赖更新后才会拿到

## 状态派生
```js
// zustand 的 selector 用法

// 写法1
const App = () => {
  const url = useStore( s => URL_HITU_DS_BASE(s.name || ''));
  // ...
}
```
