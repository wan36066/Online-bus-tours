# Online-bus-tours
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard จองตั๋วรถทัวร์ออนไลน์</title>
    <!-- Google Font (Prompt) -->
    <link href="https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- html2canvas สำหรับแปลงตั๋วเป็นรูปภาพดาวน์โหลด -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Prompt', sans-serif;
            margin: 0;
            padding: 0;
        }
        body {
            background-color: #f4f6f9;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }
        .dashboard-container {
            display: flex;
            width: 100%;
            max-width: 1100px;
            background: #fff;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.08);
            overflow: hidden;
        }
        .form-section {
            flex: 1.2;
            padding: 40px;
            background: #ffffff;
            max-height: 90vh;
            overflow-y: auto;
        }
        .form-section h2 {
            color: #1e293b;
            margin-bottom: 20px;
            font-size: 24px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .form-row {
            display: flex;
            gap: 15px;
        }
        .form-group {
            margin-bottom: 16px;
            flex: 1;
        }
        label {
            display: block;
            margin-bottom: 6px;
            font-weight: 500;
            color: #64748b;
            font-size: 14px;
        }
        input, select {
            width: 100%;
            padding: 11px 14px;
            border: 1px solid #cbd5e1;
            border-radius: 8px;
            font-size: 15px;
            outline: none;
            transition: all 0.3s;
            background-color: #fff;
        }
        input:focus, select:focus {
            border-color: #3b82f6;
            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.15);
        }
        .btn-book {
            width: 100%;
            background-color: #3b82f6;
            color: white;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            margin-top: 10px;
            transition: background-color 0.3s;
        }
        .btn-book:hover {
            background-color: #2563eb;
        }

        /* Preview / Ticket Section */
        .preview-section {
            flex: 1;
            background: #f8fafc;
            padding: 40px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            border-left: 1px solid #e2e8f0;
        }
        .placeholder-text {
            color: #94a3b8;
            text-align: center;
            font-size: 16px;
        }

        /* Ticket / A4 Slip Design */
        #ticketSlip {
            display: none;
            width: 100%;
            max-width: 420px;
            background: #ffffff;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.1);
            overflow: hidden;
            border: 1px solid #e2e8f0;
        }
        .ticket-header {
            background: #1e293b;
            color: white;
            padding: 20px;
            text-align: center;
        }
        .ticket-header h3 {
            font-size: 20px;
            margin-bottom: 4px;
        }
        .ticket-header p {
            font-size: 12px;
            color: #94a3b8;
        }
        .ticket-body {
            padding: 24px;
        }
        .ticket-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 12px;
            font-size: 14px;
        }
        .ticket-row span:first-child {
            color: #64748b;
        }
        .ticket-row span:last-child {
            font-weight: 600;
            color: #1e293b;
            text-align: right;
        }
        .ticket-divider {
            border-top: 1px dashed #cbd5e1;
            margin: 16px 0;
        }
        .ticket-footer {
            background: #f1f5f9;
            padding: 15px 24px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 15px;
            font-weight: 600;
            color: #0f172a;
        }
        
        /* Action Buttons inside Ticket */
        .ticket-actions {
            display: flex;
            gap: 10px;
            margin-top: 20px;
            width: 100%;
            max-width: 420px;
        }
        .btn-action {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 6px;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
            color: white;
            transition: opacity 0.2s;
        }
        .btn-action:hover { opacity: 0.9; }
        .btn-download { background-color: #10b981; }
        .btn-email { background-color: #8b5cf6; }

        @media (max-width: 768px) {
            .dashboard-container {
                flex-direction: column;
            }
            .preview-section {
                border-left: none;
                border-top: 1px solid #e2e8f0;
            }
        }
    </style>
</head>
<body>

<div class="dashboard-container">
    <!-- ฟอร์มกรอกข้อมูล (ฝั่งซ้าย) -->
    <div class="form-section">
        <h2>🚌 Dashboard จองตั๋วรถทัวร์</h2>
        <form id="bookingForm">
            <div class="form-group">
                <label>อีเมลผู้จอง (สำหรับรับข้อมูลตั๋ว)</label>
                <input type="email" id="email" placeholder="example@email.com" required>
            </div>

            <div class="form-row">
                <div class="form-group">
                    <label>1. ชื่อ - นามสกุล</label>
                    <input type="text" id="fullname" placeholder="ชื่อจริง นามสกุล" required>
                </div>
                <div class="form-group">
                    <label>2. เบอร์โทรศัพท์</label>
                    <input type="tel" id="phone" placeholder="0812345678" pattern="[0-9]{10}" required>
                </div>
            </div>

            <div class="form-row">
                <div class="form-group">
                    <label>5. สถานีต้นทาง</label>
                    <select id="origin" required>
                        <option value="">-- เลือกจังหวัดต้นทาง --</option>
                        <option value="พิษณุโลก" style="font-weight:bold; color:#1d4ed8;">📍 พิษณุโลก (แนะนำ)</option>
                        <option disabled>-----------------------------</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>5. สถานีปลายทาง</label>
                    <select id="destination" required>
                        <option value="">-- เลือกจังหวัดปลายทาง --</option>
                        <option value="ระยอง" style="font-weight:bold; color:#1d4ed8;">📍 ระยอง (แนะนำ)</option>
                        <option disabled>-----------------------------</option>
                    </select>
                </div>
            </div>

            <div class="form-row">
                <div class="form-group">
                    <label>6. วันที่เดินทาง</label>
                    <input type="date" id="travelDate" required>
                </div>
                <div class="form-group">
                    <label>6. เวลาเดินทาง</label>
                    <select id="travelTime" required>
                        <option value="">-- เลือกเวลา --</option>
                        <option value="06:00 น.">06:00 น.</option>
                        <option value="09:30 น.">09:30 น.</option>
                        <option value="13:00 น.">13:00 น.</option>
                        <option value="18:30 น.">18:30 น.</option>
                        <option value="21:00 น.">21:00 น.</option>
                    </select>
                </div>
            </div>

            <div class="form-row">
                <div class="form-group">
                    <label>3. เลขที่นั่งตั๋ว</label>
                    <input type="text" id="seatNumber" placeholder="เช่น A1, B12" required>
                </div>
                <div class="form-group">
                    <label>4. ราคาตั๋ว (บาท)</label>
                    <input type="number" id="price" placeholder="เช่น 650" min="0" required>
                </div>
            </div>

            <button type="submit" class="btn-book">7. ยืนยันและออกตั๋วโดยสาร</button>
        </form>
    </div>

    <!-- ส่วนแสดงผลตั๋ว/สลิป (ฝั่งขวา) -->
    <div class="preview-section">
        <div id="placeholderText" class="placeholder-text">
            📋 กรอกข้อมูลการจองด้านซ้าย<br>แล้วกด "ยืนยันและออกตั๋ว" เพื่อดูสลิปตั๋วโดยสาร
        </div>

        <!-- หน้าตาตั๋วที่จะแสดงผลและดาวน์โหลด -->
        <div id="ticketSlip">
            <div class="ticket-header">
                <h3>E-TICKET BUS PASS</h3>
                <p>ใบยืนยันการจองตั๋วรถทัวร์ออนไลน์</p>
            </div>
            <div class="ticket-body">
                <div class="ticket-row">
                    <span>ผู้โดยสาร:</span>
                    <span id="resName">-</span>
                </div>
                <div class="ticket-row">
                    <span>เบอร์โทรศัพท์:</span>
                    <span id="resPhone">-</span>
                </div>
                <div class="ticket-row">
                    <span>เส้นทาง:</span>
                    <span id="resRoute">-</span>
                </div>
                <div class="ticket-row">
                    <span>วัน-เวลา:</span>
                    <span id="resDateTime">-</span>
                </div>
                <div class="ticket-divider"></div>
                <div class="ticket-row">
                    <span>เลขที่นั่ง:</span>
                    <span id="resSeat" style="color: #2563eb; font-size: 16px;">-</span>
                </div>
            </div>
            <div class="ticket-footer">
                <span>ราคารวมทั้งสิ้น</span>
                <span id="resPrice" style="color: #10b981;">0 บาท</span>
            </div>
        </div>

        <!-- ปุ่มดาวน์โหลด / ส่งอีเมล -->
        <div id="ticketActions" class="ticket-actions" style="display: none;">
            <button class="btn-action btn-download" onclick="downloadTicket()">📥 บันทึกลงมือถือ</button>
            <button class="btn-action btn-email" onclick="sendEmail()">✉️ ส่งไปทางอีเมล</button>
        </div>
    </div>
</div>

<script>
    // รายชื่อ 77 จังหวัดในประเทศไทย
    const provinces = [
        "กระบี่", "กรุงเทพมหานคร", "กาญจนบุรี", "กาฬสินธุ์", "กำแพงเพชร", 
        "ขอนแก่น", "จันทบุรี", "ฉะเชิงเทรา", "ชลบุรี", "ชัยนาท", 
        "ชัยภูมิ", "ชุมพร", "ตรัง", "ตราด", "ตาก", 
        "นครนายก", "นครปฐม", "นครพนม", "นครราชสีมา", "นครศรีธรรมราช", 
        "นครสวรรค์", "นนทบุรี", "นราธิวาส", "น่าน", "บึงกาฬ", 
        "บุรีรัมย์", "ปทุมธานี", "ประจวบคีรีขันธ์", "ปราจีนบุรี", "ปัตตานี", 
        "พระนครศรีอยุธยา", "พะเยา", "พังงา", "พัทลุง", "พิจิตร", 
        "พิษณุโลก", "เพชรบุรี", "เพชรบูรณ์", "แพร่", "ภูเก็ต", 
        "มหาสารคาม", "มุกดาหาร", "แม่ฮ่องสอน", "ยโสธร", "ยะลา", 
        "ร้อยเอ็ด", "ระนอง", "ระยอง", "ราชบุรี", "ลพบุรี", 
        "ลำปาง", "ลำพูน", "เลย", "ศรีสะเกษ", "สกลนคร", 
        "สงขลา", "สตูล", "สมุทรปราการ", "สมุทรสงคราม", "สมุทรสาคร", 
        "สระแก้ว", "สระบุรี", "สิงห์บุรี", "สุโขทัย", "สุพรรณบุรี", 
        "สุราษฎร์ธานี", "สุรินทร์", "หนองคาย", "หนองบัวลำภู", "อ่างทอง", 
        "อำนาจเจริญ", "อุดรธานี", "อุตรดิตถ์", "อุทัยธานี", "อุบลราชธานี"
    ];

    const originSelect = document.getElementById('origin');
    const destSelect = document.getElementById('destination');

    provinces.forEach(prov => {
        if (prov !== "พิษณุโลก") {
            let opt1 = document.createElement('option');
            opt1.value = prov;
            opt1.textContent = prov;
            originSelect.appendChild(opt1);
        }
        if (prov !== "ระยอง") {
            let opt2 = document.createElement('option');
            opt2.value = prov;
            opt2.textContent = prov;
            destSelect.appendChild(opt2);
        }
    });

    let bookingData = {};

    document.getElementById('bookingForm').addEventListener('submit', function(e) {
        e.preventDefault();

        const origin = document.getElementById('origin').value;
        const destination = document.getElementById('destination').value;

        if (origin === destination) {
            alert('สถานีต้นทางและปลายทางต้องไม่ซ้ำกัน กรุณาเลือกใหม่อีกครั้ง');
            return;
        }

        bookingData = {
            email: document.getElementById('email').value,
            name: document.getElementById('fullname').value,
            phone: document.getElementById('phone').value,
            origin: origin,
            destination: destination,
            date: document.getElementById('travelDate').value,
            time: document.getElementById('travelTime').value,
            seat: document.getElementById('seatNumber').value,
            price: Number(document.getElementById('price').value).toLocaleString()
        };

        // แสดงผลลงในตั๋ว
        document.getElementById('resName').innerText = bookingData.name;
        document.getElementById('resPhone').innerText = bookingData.phone;
        document.getElementById('resRoute').innerText = `${bookingData.origin} ➔ ${bookingData.destination}`;
        document.getElementById('resDateTime').innerText = `${bookingData.date} (${bookingData.time})`;
        document.getElementById('resSeat').innerText = bookingData.seat;
        document.getElementById('resPrice').innerText = `${bookingData.price} บาท`;

        // สลับมุมมองแสดงตั๋ว
        document.getElementById('placeholderText').style.display = 'none';
        document.getElementById('ticketSlip').style.display = 'block';
        document.getElementById('ticketActions').style.display = 'flex';
    });

    function downloadTicket() {
        const ticketElement = document.getElementById('ticketSlip');
        html2canvas(ticketElement, { scale: 2 }).then(canvas => {
            const link = document.createElement('a');
            link.download = `Bus-Ticket-${bookingData.seat}.png`;
            link.href = canvas.toDataURL('image/png');
            link.click();
        });
    }

    function sendEmail() {
        const subject = encodeURIComponent(`ยืนยันการจองตั๋วรถทัวร์ เส้นทาง ${bookingData.origin} - ${bookingData.destination}`);
        const body = encodeURIComponent(
            `เรียน คุณ ${bookingData.name},\n\nข้อมูลการจองตั๋วรถทัวร์ของคุณ:\n` +
            `- เส้นทาง: ${bookingData.origin} ไปยัง ${bookingData.destination}\n` +
            `- วันเวลาเดินทาง: ${bookingData.date} เวลา ${bookingData.time}\n` +
            `- เลขที่นั่ง: ${bookingData.seat}\n` +
            `- ราคา: ${bookingData.price} บาท\n` +
            `- เบอร์โทรติดต่อ: ${bookingData.phone}\n\nขอบคุณที่ใช้บริการครับ`
        );
        window.location.href = `mailto:${bookingData.email}?subject=${subject}&body=${body}`;
    }
</script>

</body>
</html>
