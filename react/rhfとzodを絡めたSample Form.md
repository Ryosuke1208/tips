## react-hook-formとzodのフォーム

```ts
import { useForm } from "react-hook-form";  // フォームの状態管理とバリデーションのために useForm をインポート
import "./App.css";  // スタイルを適用するために CSS ファイルをインポート
import React from "react";  // Reactコンポーネントを使用するためにReactをインポート
import { z } from "zod";  // Zod ライブラリをインポート
import { zodResolver } from "@hookform/resolvers/zod";  // zod のスキーマを react-hook-form に組み込むための zodResolver をインポート

// バリデーションスキーマをZodで定義
const validationSchema = z.object({
  name: z.string().min(1, "名前は必須です"),  // 名前は空でない文字列
  email: z.string().email("有効なメールアドレスを入力してください"),  // メールアドレスは有効な形式
  password: z.string().min(6, "パスワードは6文字以上で入力してください"),  // パスワードは6文字以上
});

interface LoginForm {
  name: string;  // name フィールドの型は string
  email: string;  // email フィールドの型は string
  password: string;  // password フィールドの型は string
}

function App() {
  const {
    register,  // フォームフィールドにバリデーションを登録するための関数
    handleSubmit,  // フォーム送信時の処理をラップするための関数
    formState: { errors },  // フォーム内のバリデーションエラーを保持するオブジェクト
  } = useForm<LoginForm>({
    mode: "onChange",  // フォームの値が変わるたびにバリデーションを実行
    resolver: zodResolver(validationSchema),  // zod のバリデーションスキーマを使用
  });

  const onSubmit = (data: LoginForm) => {
    console.log(data);  // フォーム送信時に入力データをコンソールに表示
  };

  return (
    <div className="form-container">  {/* フォーム全体を囲むコンテナ */}
      <h1>Login Form</h1>  {/* フォームのタイトルを表示 */}
      <form onSubmit={handleSubmit(onSubmit)}>  {/* フォームの送信時にバリデーションと onSubmit を実行 */}
        <label htmlFor="名前">名前</label>  {/* 名前フィールドのラベル */}
        <input id="name" type="text" {...register("name")} />  {/* 名前の入力フィールド、バリデーション付き */}
        <p>{errors.name?.message as React.ReactNode}</p>  {/* 名前フィールドにエラーがある場合、そのメッセージを表示 */}

        <label htmlFor="メールアドレス">メールアドレス</label>  {/* メールアドレスフィールドのラベル */}
        <input id="email" type="email" {...register("email")} />  {/* メールアドレスの入力フィールド、バリデーション付き */}
        <p>{errors.email?.message as React.ReactNode}</p>  {/* メールアドレスフィールドにエラーがある場合、そのメッセージを表示 */}

        <label htmlFor="パスワード">パスワード</label>  {/* パスワードフィールドのラベル */}
        <input type="password" id="password" {...register("password")} />  {/* パスワードの入力フィールド、バリデーション付き */}
        <p>{errors.password?.message as React.ReactNode}</p>  {/* パスワードフィールドにエラーがある場合、そのメッセージを表示 */}

        <button type="submit">送信</button>  {/* 送信ボタン */}
      </form>
    </div>
  );
}

export default App;  // Appコンポーネントをエクスポート


```