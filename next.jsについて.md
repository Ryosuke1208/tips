#MUIのprops＋正規表現が使えるカスタムコンポーネントの作成方法

```typescript
'use client'

import React, { useState } from 'react';
import TextField, { TextFieldProps } from '@mui/material/TextField';

interface CustomProps {
  regex: string; // 正規表現を文字列として受け取る
}

type CustomTextFieldProps = TextFieldProps & CustomProps;

const CustomTextField: React.FC<CustomTextFieldProps> = ({ regex, ...props }) => {
  const [inputValue, setInputValue] = useState('');

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const newValue = e.target.value;
    const regexObj = new RegExp(regex); // 文字列からRegExpオブジェクトを作成
    console.log(regexObj)
    // 入力が正規表現に一致するか確認
    if (regexObj.test(newValue) || newValue === '') {
      setInputValue(newValue);
    }
  };

  return <TextField value={inputValue} onChange={handleChange} {...props} />;
};

export default CustomTextField;
```


```typescript
import CustomTextField from "@/components/CustomTextField/CustomTextField";

const NumberInputLimit = () => {
  return(
    <CustomTextField regex='^\d*$' />
  )
};

export default NumberInputLimit;
```

```typescript
import NumberInputLimit from "@/components/CustomTextField/CustomTextField";
import { Annie_Use_Your_Telescope } from "next/font/google";

const PAGE = () => {
  return(
    <NumberInputLimit label="aiueo" regex='^\d*$' />
  )
};

export default PAGE;
```