<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Калькулятор натяжного потолка</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 text-gray-800 min-h-screen flex items-center justify-center p-4">
  <div class="bg-white shadow-lg rounded-xl p-6 w-full max-w-xl">
    <h1 class="text-2xl font-bold mb-6 text-center">Калькулятор стоимости потолка</h1>

    <form id="form" onsubmit="handleSubmit(event)">
      <label class="block mb-4">
        <span class="block font-medium mb-1">Площадь потолка (м²):</span>
        <input type="number" id="area" value="12" class="w-full border border-gray-300 rounded-lg p-2">
      </label>

      <label class="block mb-4">
        <span class="block font-medium mb-1">Тип полотна:</span>
        <select id="materialType" onchange="updateSubtypes()" class="w-full border border-gray-300 rounded-lg p-2">
          <option value="">-- Выберите --</option>
          <option value="fabric">Тканевое</option>
          <option value="pvc">ПВХ</option>
        </select>
      </label>

      <label class="block mb-4">
        <span class="block font-medium mb-1">Подтип полотна:</span>
        <select id="subtype" class="w-full border border-gray-300 rounded-lg p-2">
          <option value="">-- Сначала выберите тип --</option>
        </select>
      </label>

      <label class="block mb-4">
        <span class="block font-medium mb-1">Количество углов:</span>
        <input type="number" id="corners" value="4" class="w-full border border-gray-300 rounded-lg p-2">
      </label>

      <label class="block mb-4">
        <span class="block font-medium mb-1">Количество люстр:</span>
        <input type="number" id="chandeliers" value="1" class="w-full border border-gray-300 rounded-lg p-2">
      </label>

      <label class="block mb-4">
        <span class="block font-medium mb-1">Количество труб:</span>
        <input type="number" id="pipes" value="2" class="w-full border border-gray-300 rounded-lg p-2">
      </label>

      <label class="block mb-4">
        <span class="block font-medium mb-1">Ваше имя:</span>
        <input type="text" id="name" class="w-full border border-gray-300 rounded-lg p-2">
      </label>

      <label class="block mb-4">
        <span class="block font-medium mb-1">Телефон для связи:</span>
        <input type="tel" id="phone" value="89165028276" readonly class="w-full border border-gray-300 rounded-lg p-2 bg-gray-100 cursor-not-allowed">
      </label>

      <label class="block mb-4">
        <span class="block font-medium mb-1">Прикрепить фото (опционально):</span>
        <input type="file" id="photo" accept="image/*" class="w-full">
      </label>

      <button type="submit" class="w-full bg-blue-600 text-white font-semibold py-2 px-4 rounded-lg hover:bg-blue-700 transition">Рассчитать</button>
    </form>

    <div id="result" class="mt-6 text-lg font-bold text-center"></div>
    <div id="confirmation" class="mt-4 text-green-600 text-center hidden">Заявка отправлена! Мы свяжемся с вами по номеру <strong>89165028276</strong>.</div>
  </div>

  <script>
    const prices = {
      fabric: {
        descor: 900
      },
      pvc: {
        msd: 600,
        msd_premium: 700,
        bauf: 750
      }
    };

    function updateSubtypes() {
      const materialType = document.getElementById('materialType').value;
      const subtypeSelect = document.getElementById('subtype');
      subtypeSelect.innerHTML = '';

      if (!materialType || !prices[materialType]) return;

      const subtypes = Object.keys(prices[materialType]);
      subtypes.forEach(key => {
        const option = document.createElement('option');
        option.value = key;
        option.text = key.toUpperCase().replace('_', ' ');
        subtypeSelect.appendChild(option);
      });
    }

    function calculate() {
      const area = parseFloat(document.getElementById('area').value) || 0;
      const materialType = document.getElementById('materialType').value;
      const subtype = document.getElementById('subtype').value;
      const corners = parseInt(document.getElementById('corners').value) || 0;
      const chandeliers = parseInt(document.getElementById('chandeliers').value) || 0;
      const pipes = parseInt(document.getElementById('pipes').value) || 0;

      if (!prices[materialType] || !prices[materialType][subtype]) {
        document.getElementById('result').innerText = 'Пожалуйста, выберите тип и подтип полотна';
        return null;
      }

      const typePrice = prices[materialType][subtype];
      const base = area * typePrice;
      const cornersCost = corners * 100;
      const chandelierCost = chandeliers * 600;
      const pipeCost = pipes * 400;

      return base + cornersCost + chandelierCost + pipeCost;
    }

    function handleSubmit(event) {
      event.preventDefault();
      const total = calculate();
      if (total === null) return;

      document.getElementById('result').innerText = `Итоговая стоимость: ${total.toLocaleString()} ₽`;
      document.getElementById('confirmation').classList.remove('hidden');
    }
  </script>
</body>
</html>
