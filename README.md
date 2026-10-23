<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>คำนวณค่าบริการและอุปกรณ์ไฟฟ้า</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen py-10 px-4">
    <div class="max-w-xl mx-auto bg-white rounded-xl shadow-md overflow-hidden p-6">
        <h2 class="text-2xl font-bold text-center text-gray-800 mb-2">คำนวณค่าบริการอุปกรณ์ไฟฟ้า</h2>
        <p class="text-center text-gray-500 text-sm mb-6">ระบบคำนวณราคาค่าบริการและอุปกรณ์สำหรับช่างและลูกค้า</p>
        
        <!-- กล่องแสดงขั้นตอนการใช้งานในแอป -->
        <div class="bg-blue-50 border border-blue-200 rounded-xl p-4 mb-6 text-sm text-blue-900">
            <p class="font-bold mb-1 flex items-center gap-1">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 inline" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>
                ขั้นตอนการใช้งาน:
            </p>
            <ol class="list-decimal list-inside space-y-1 text-xs text-blue-800">
                <li>เลือกรายการอุปกรณ์ไฟฟ้า และกดปุ่ม <span class="bg-white px-1.5 py-0.5 rounded border border-blue-200 font-bold">+</span> หรือ <span class="bg-white px-1.5 py-0.5 rounded border border-blue-200 font-bold">-</span> เพื่อปรับจำนวนชิ้น (เพิ่มรายการได้ด้วยปุ่มเขียวด้านล่าง)</li>
                <li>เลือกเปิด/ปิด <span class="font-semibold">ค่าปลดสับ (570 บาท)</span> ตามหน้างานจริง</li>
                <li>ปรับ <span class="font-semibold">จำนวนหน่วยตรวจสอบแก้ไข</span> (285 บาท/หน่วย)</li>
                <li>ตรวจสอบยอดรวมสุทธิพร้อม VAT 7% ด้านล่างสุดได้ทันที</li>
            </ol>
        </div>

        <div class="space-y-6">
            <!-- ส่วนเลือกรายการอุปกรณ์ (หลายรายการ) -->
            <div>
                <div class="flex justify-between items-center mb-2">
                    <label class="block text-gray-700 font-medium">รายการอุปกรณ์ไฟฟ้า (เลือกได้มากกว่า 1)</label>
                    <button type="button" onclick="addRow()" class="bg-green-600 text-white text-sm px-3 py-1.5 rounded-lg hover:bg-green-700 transition font-medium">+ เพิ่มอุปกรณ์</button>
                </div>
                
                <div id="itemRows" class="space-y-3">
                    <!-- แถวเริ่มต้น 1 แถว -->
                    <div class="item-row bg-gray-50 p-3 rounded-lg border space-y-3">
                        <div class="flex gap-2 items-center">
                            <select class="fuse-select flex-1 px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 bg-white text-sm font-medium" onchange="calculate()">
                                <option value="0">-- เลือกอุปกรณ์ไฟฟ้า --</option>
                            </select>
                            <button type="button" onclick="removeRow(this)" class="text-red-500 hover:text-red-700 px-2 font-bold text-xl" title="ลบรายการ">×</button>
                        </div>
                        
                        <!-- ปุ่มกดบวก/ลบ จำนวน -->
                        <div class="flex items-center justify-between bg-white p-2 rounded-lg border">
                            <span class="text-xs text-gray-500 font-medium">จำนวนชิ้น:</span>
                            <div class="flex items-center border rounded-lg bg-gray-50 overflow-hidden shadow-sm">
                                <button type="button" onclick="changeQty(this, -1)" class="px-4 py-1.5 bg-gray-200 hover:bg-gray-300 text-gray-800 font-bold text-sm transition">-</button>
                                <span class="fuse-qty px-4 py-1.5 text-center font-semibold text-sm min-w-[40px] inline-block">1</span>
                                <button type="button" onclick="changeQty(this, 1)" class="px-4 py-1.5 bg-gray-200 hover:bg-gray-300 text-gray-800 font-bold text-sm transition">+</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- ค่าบริการอื่นๆ -->
            <div class="space-y-4 pt-4 border-t">
                <div class="flex items-center justify-between bg-gray-50 p-3 rounded-lg border">
                    <div>
                        <span class="font-medium text-gray-700">ค่าปลดสับ</span>
                        <p class="text-xs text-gray-500">570 บาท / ครั้ง</p>
                    </div>
                    <input type="checkbox" id="includeSwitching" checked onchange="calculate()" class="w-5 h-5 text-blue-600 rounded focus:ring-blue-500">
                </div>

                <div>
                    <label class="block text-gray-700 font-medium mb-1">จำนวนหน่วยตรวจสอบแก้ไข (285 บาท/หน่วย)</label>
                    <div class="flex items-center justify-between border rounded-lg bg-white p-2 shadow-sm">
                        <span class="text-xs text-gray-500 font-medium">ปรับจำนวนหน่วย:</span>
                        <div class="flex items-center border rounded-lg bg-gray-50 overflow-hidden">
                            <button type="button" onclick="changeUnitQty(-1)" class="px-4 py-1.5 bg-gray-200 hover:bg-gray-300 text-gray-800 font-bold text-sm transition">-</button>
                            <span id="units" class="px-4 py-1.5 text-center font-semibold text-sm min-w-[40px] inline-block">1</span>
                            <button type="button" onclick="changeUnitQty(1)" class="px-4 py-1.5 bg-gray-200 hover:bg-gray-300 text-gray-800 font-bold text-sm transition">+</button>
                        </div>
                    </div>
                </div>

                <!-- ตัวเลือก VAT 7% -->
                <div class="flex items-center justify-between bg-gray-50 p-3 rounded-lg border">
                    <div>
                        <span class="font-medium text-gray-700">ภาษีมูลค่าเพิ่ม (VAT 7%)</span>
                    </div>
                    <input type="checkbox" id="includeVat" checked onchange="calculate()" class="w-5 h-5 text-blue-600 rounded focus:ring-blue-500">
                </div>
            </div>
        </div>

        <!-- ส่วนแสดงผลลัพธ์ -->
        <div class="mt-8 border-t pt-4 space-y-2">
            <div class="flex justify-between text-gray-600">
                <span>ค่าอุปกรณ์รวม:</span>
                <span id="materialCost">0 บาท</span>
            </div>
            <div class="flex justify-between text-gray-600">
                <span>ค่าปลดสับ:</span>
                <span id="switchingCost">570 บาท</span>
            </div>
            <div class="flex justify-between text-gray-600">
                <span>ค่าตรวจสอบแก้ไข:</span>
                <span id="inspectionCost">285 บาท</span>
            </div>
            <div class="flex justify-between text-gray-600 border-t pt-2">
                <span>ยอดรวมก่อน VAT:</span>
                <span id="subTotalCost">855 บาท</span>
            </div>
            <div class="flex justify-between text-gray-600">
                <span>VAT 7%:</span>
                <span id="vatCost">59.85 บาท</span>
            </div>
            <div class="flex justify-between text-xl font-bold text-gray-800 mt-2 bg-blue-50 p-3 rounded-lg">
                <span>ยอดรวมสุทธิทั้งสิ้น:</span>
                <span id="totalCost" class="text-blue-600">914.85 บาท</span>
            </div>
        </div>
    </div>

    <script>
        const equipmentList = [
            { name: "DROPOUT FUSE CUTOUT COVERS", price: 938 },
            { name: "ฟิวส์ลิงค์ 22เควี 65 แอมป์", price: 286 },
            { name: "ฟิวส์ลิงค์ 22เควี 40 แอมป์", price: 99 },
            { name: "ฟิวส์ลิงค์ 22เควี 25 แอมป์", price: 89 },
            { name: "ฟิวส์ลิงค์ 22เควี 15 แอมป์", price: 74 },
            { name: "ฟิวส์ลิงค์ 22เควี 10 แอมป์", price: 81 },
            { name: "ฟิวส์ลิงค์ 22เควี 8 แอมป์", price: 72 },
            { name: "ฟิวส์ลิงค์ 22เควี 5-6 แอมป์", price: 73 },
            { name: "ฟิวส์ลิงค์ 22เควี 3 แอมป์", price: 69 },
            { name: "LT.ฟิวส์สวิต์แรงต่ำ 1x400 แอมป์", price: 868 },
            { name: "เอชอาร์ซี ฟิวส์แรงต่ำ 400 แอมป์", price: 363 },
            { name: "เอชอาร์ซี ฟิวส์แรงต่ำ 315 แอมป์", price: 283 },
            { name: "เอชอาร์ซี ฟิวส์แรงต่ำ 250 แอมป์", price: 170 },
            { name: "เอชอาร์ซี ฟิวส์แรงต่ำ 200 แอมป์", price: 161 },
            { name: "เอชอาร์ซี ฟิวส์แรงต่ำ 150-160 แอมป์", price: 150 },
            { name: "เอชอาร์ซี ฟิวส์แรงต่ำ 100 แอมป์", price: 138 },
            { name: "เอชอาร์ซี ฟิวส์แรงต่ำ 80 แอมป์", price: 118 },
            { name: "เอชอาร์ซี ฟิวส์แรงต่ำ 50 แอมป์", price: 99 },
            { name: "กระบอกฟิวส์ 22K 100 A", price: 1260 },
            { name: "ดรอฟเอาท์ฟิวส์ 22 เควี 100 แอมป์", price: 3702 },
            { name: "ล่อฟ้า 20-21 เควี 5กิโลแอมป์", price: 1085 },
            { name: "ลูกถ้วยแขวนแบบ ก(แบบ 521) มอก.354", price: 471 },
            { name: "ลูกถ้วยไลน์โพสท์ไทพ์ 22 เควี", price: 1144 },
            { name: "หางปลา เจาะรู 185 ต.มม.", price: 49 },
            { name: "หางปลา 95 ต.มม.", price: 33 },
            { name: "หางปลาสำหรับสายอลูมิเนียม 50 ต.มม.", price: 15 },
            { name: "หลอดต่อสายชนิดบีบ 50/8 ต.มม.", price: 78 },
            { name: "หลอดต่อสายชนิดบีบ รับแรงดึงสายอลูมิเนียม 185 ต.มม.", price: 89 },
            { name: "หลอดต่อสายชนิดบีบ รับแรงดึงสายอลูมิเนียม 120 ต.มม.", price: 84 },
            { name: "หลอดต่อสายชนิดบีบ รับแรงดึงสายอลูมิเนียม 95 ต.มม.", price: 39 },
            { name: "หลอดต่อสายชนิดบีบ รับแรงดึงสายอลูมิเนียม 50 ต.มม.", price: 22 },
            { name: "ฮอทไลน์แคล้มป์ 35-185 ต.มม.", price: 852 },
            { name: "ฮอทไลน์แคล้มป์ 50-120 ต.มม.", price: 276 },
            { name: "ฮอทไลน์แคล้มป์ 25-50 ต.มม.", price: 147 },
            { name: "เบลแคล้มป์ฮ้อทไลน์ 70-185 ต.มม.", price: 1050 },
            { name: "เบลแคล้มป์ฮ้อทไลน์ 35-70 ต.มม.", price: 910 },
            { name: "พีจี คอนเน็คเตอร์ 3 สลัก 70 ต.มม.", price: 94 },
            { name: "พีจี 25-95 ต.มม.", price: 28 },
            { name: "ปรีฟอร์มเข้าปลายสาย SAC 22KV 185 ต.มม.", price: 192 },
            { name: "ปรีฟอร์มเข้าปลายสาย PIC 120 ต.มม. และ SAC 50 ต.มม.", price: 110 },
            { name: "เทปพันเคเบิ้ลอากาศแรงสูง ขนาด3/4x30 นิ้ว", price: 47 },
            { name: "เทปไฟฟ้าพีวีซี.", price: 8 },
            { name: "สายอลูมิเนียม 95 ต.มม.", price: 71 },
            { name: "สายอลูมิเนียม 50 ต.มม.", price: 47 },
            { name: "สาย SAC 185 ต.มม.", price: 227 },
            { name: "สาย SAC 50 ต.มม.", price: 115 },
            { name: "สายอลูมิเนียมแกนเหล็ก 50/8 ต.มม.", price: 35 },
            { name: "สายอลูมิเนียมเปลือย 185 ต.มม.", price: 66 },
            { name: "สายอลูมิเนียมเปลือย 120 ต.มม.", price: 27 },
            { name: "สายอลูมิเนียมเปลือย 95 ต.มม.", price: 30 }
        ];

        function populateSelect(selectElement) {
            equipmentList.forEach(item => {
                const option = document.createElement('option');
                option.value = item.price;
                option.text = `${item.name} (${item.price.toLocaleString()} บาท)`;
                selectElement.appendChild(option);
            });
        }

        populateSelect(document.querySelector('.fuse-select'));

        function addRow() {
            const container = document.getElementById('itemRows');
            const newRow = document.createElement('div');
            newRow.className = 'item-row bg-gray-50 p-3 rounded-lg border space-y-3';
            newRow.innerHTML = `
                <div class="flex gap-2 items-center">
                    <select class="fuse-select flex-1 px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 bg-white text-sm font-medium" onchange="calculate()">
                        <option value="0">-- เลือกอุปกรณ์ไฟฟ้า --</option>
                    </select>
                    <button type="button" onclick="removeRow(this)" class="text-red-500 hover:text-red-700 px-2 font-bold text-xl" title="ลบรายการ">×</button>
                </div>
                <div class="flex items-center justify-between bg-white p-2 rounded-lg border">
                    <span class="text-xs text-gray-500 font-medium">จำนวนชิ้น:</span>
                    <div class="flex items-center border rounded-lg bg-gray-50 overflow-hidden shadow-sm">
                        <button type="button" onclick="changeQty(this, -1)" class="px-4 py-1.5 bg-gray-200 hover:bg-gray-300 text-gray-800 font-bold text-sm transition">-</button>
                        <span class="fuse-qty px-4 py-1.5 text-center font-semibold text-sm min-w-[40px] inline-block">1</span>
                        <button type="button" onclick="changeQty(this, 1)" class="px-4 py-1.5 bg-gray-200 hover:bg-gray-300 text-gray-800 font-bold text-sm transition">+</button>
                    </div>
                </div>
            `;
            container.appendChild(newRow);
            populateSelect(newRow.querySelector('.fuse-select'));
            calculate();
        }

        function changeQty(button, delta) {
            const container = button.closest('.item-row');
            const span = container.querySelector('.fuse-qty');
            let currentValue = parseInt(span.innerText) || 1;
            let newValue = currentValue + delta;
            if (newValue < 1) newValue = 1;
            span.innerText = newValue;
            calculate();
        }

        function changeUnitQty(delta) {
            const span = document.getElementById('units');
            let currentValue = parseInt(span.innerText) || 0;
            let newValue = currentValue + delta;
            if (newValue < 0) newValue = 0;
            span.innerText = newValue;
            calculate();
        }

        function removeRow(button) {
            const row = button.closest('.item-row');
            const totalRows = document.querySelectorAll('.item-row').length;
            if (totalRows > 1) {
                row.remove();
            } else {
                row.querySelector('.fuse-select').value = "0";
                row.querySelector('.fuse-qty').innerText = "1";
            }
            calculate();
        }

        function calculate() {
            let totalMaterial = 0;
            const rows = document.querySelectorAll('.item-row');
            
            rows.forEach(row => {
                const price = parseInt(row.querySelector('.fuse-select').value) || 0;
                const qty = parseInt(row.querySelector('.fuse-qty').innerText) || 0;
                totalMaterial += price * qty;
            });

            const includeSwitching = document.getElementById('includeSwitching').checked;
            const units = parseInt(document.getElementById('units').innerText) || 0;
            const includeVat = document.getElementById('includeVat').checked;

            const switchingPrice = includeSwitching ? 570 : 0;
            const inspectionPrice = units * 285;
            
            const subTotal = totalMaterial + switchingPrice + inspectionPrice;
            const vat = includeVat ? subTotal * 0.07 : 0;
            const total = subTotal + vat;

            document.getElementById('materialCost').innerText = totalMaterial.toLocaleString('th-TH', {minimumFractionDigits: 2, maximumFractionDigits: 2}) + ' บาท';
            document.getElementById('switchingCost').innerText = switchingPrice.toLocaleString('th-TH', {minimumFractionDigits: 2, maximumFractionDigits: 2}) + ' บาท';
            document.getElementById('inspectionCost').innerText = inspectionPrice.toLocaleString('th-TH', {minimumFractionDigits: 2, maximumFractionDigits: 2}) + ' บาท';
            document.getElementById('subTotalCost').innerText = subTotal.toLocaleString('th-TH', {minimumFractionDigits: 2, maximumFractionDigits: 2}) + ' บาท';
            document.getElementById('vatCost').innerText = vat.toLocaleString('th-TH', {minimumFractionDigits: 2, maximumFractionDigits: 2}) + ' บาท';
            document.getElementById('totalCost').innerText = total.toLocaleString('th-TH', {minimumFractionDigits: 2, maximumFractionDigits: 2}) + ' บาท';
        }

        calculate();
    </script>
</body>
</html>