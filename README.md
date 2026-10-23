<!DOCTYPE html>
<html lang="tg">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BozoriTJ — Бозори Тоҷикистон</title>

  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f4f6f5;
      color: #222;
    }

    header {
      background: #087f5b;
      color: white;
      padding: 22px 12px;
      text-align: center;
    }

    header h1 {
      margin: 0 0 8px;
    }

    header p {
      margin: 0;
    }

    .search, input, textarea, select {
      width: 100%;
      padding: 12px;
      margin: 7px 0;
      border: 1px solid #ddd;
      border-radius: 7px;
      font-size: 16px;
    }

    .search {
      display: block;
      width: 90%;
      max-width: 600px;
      margin: 20px auto;
      background: white;
    }

    main {
      max-width: 1000px;
      margin: auto;
      padding: 15px;
    }

    .panel, .product {
      background: white;
      padding: 16px;
      margin-bottom: 16px;
      border-radius: 10px;
      box-shadow: 0 2px 8px #0001;
    }

    button {
      background: #087f5b;
      color: white;
      border: 0;
      border-radius: 7px;
      padding: 12px 16px;
      margin: 5px 0;
      cursor: pointer;
      font-size: 15px;
    }

    button:disabled {
      opacity: 0.6;
      cursor: wait;
    }

    .secondary {
      background: #555;
    }

    #products {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
      gap: 14px;
    }

    .product img {
      width: 100%;
      height: 170px;
      object-fit: contain;
      border-radius: 7px;
      background: #f8f8f8;
    }

    .price {
      color: #087f5b;
      font-weight: bold;
      font-size: 18px;
    }

    #status {
      white-space: pre-wrap;
      overflow-wrap: anywhere;
      padding: 8px 0;
    }

    footer {
      text-align: center;
      padding: 20px;
      color: #666;
    }

    [hidden] {
      display: none !important;
    }
  </style>
</head>

<body>
  <header>
    <h1>BozoriTJ</h1>
    <p>Бозори Тоҷикистон — хариду фурӯши маҳсулот</p>
  </header>

  <input
    id="search"
    class="search"
    type="search"
    placeholder="Маҳсулотро ҷустуҷӯ кун..."
    aria-label="Ҷустуҷӯи маҳсулот"
  >

  <main>
    <!-- Қисми рӯйхати маҳсулот ва администратор
         бояд дар идомаи ҳамин файл бошад. -->
  </main>
</body>
</html>
