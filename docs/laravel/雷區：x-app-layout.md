
### x-app-layout 的原理是什麼

● `<x-app-layout>` 的原理：

  1. Blade Components 系統

  `<x-app-layout>` 是 Laravel 的 Blade Components 功能，它是一種更現代的模板組件化方式。

  2. 命名規則

  - `<x-app-layout>` 對應到 `resources/views/layouts/app.blade.php`
  - `<x-button>` 對應到 `resources/views/components/button.blade.php`
  - `<x-forms.input>` 對應到 `resources/views/components/forms/input.blade.php`

  2. `{{ $slot }}` 變數

  在 `layouts/app.blade.php` 中的 `{{ $slot }}` 是一個特殊變數，會被替換成：
  ```
  <x-app-layout>
      這裡的內容會變成 $slot 的值
  </x-app-layout>
  ```

  3. 具名 slots

  你也可以傳遞具名的內容區塊：
  ```
  <x-app-layout>
      <x-slot name="header">
          <h1>這會傳給 $header 變數</h1>
      </x-slot>

      <x-slot name="scripts">
          <script>console.log('這會傳給 $scripts 變數')</script>
      </x-slot>

      這裡是主要內容，會傳給 $slot
  </x-app-layout>
  ```

  4. 對比傳統方式

  舊的 @extends/@section 方式：
```
@extends('layouts.app')

@section('content')
    內容
@endsection
```

  新的 Component 方式：
```
<x-app-layout>
    內容
</x-app-layout>
```

5. 優點

  - 更直觀，像寫 HTML 標籤
  - 可以傳遞參數：`<x-button color="red" size="large">`
  - 更好的 IDE 支援
  - 可以寫 PHP 類別來處理邏輯
  - Laravel 8+ 預設使用 Component 方式，所以你的專案用這種語法是正常的！