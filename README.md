# poc-privacy-preserving-voc-data

POC สำหรับปกปิดข้อมูลส่วนบุคคล (PII) ในข้อความ VOC ภาษาไทยด้วย NER และสรุปประเด็นด้วย OpenAI API

## ความสามารถหลัก
- ตรวจจับและไฮไลต์ข้อมูลที่มีโอกาสเป็นข้อมูลส่วนบุคคลในข้อความภาษาไทย
- แปลงข้อความเป็นรูปแบบ anonymized/masked
- สรุปประเด็นปัญหาและวิเคราะห์ sentiment จากข้อความที่ปกปิดข้อมูลแล้ว
- มี prototype UI ด้วย Gradio ผ่าน Jupyter Notebook

## โครงสร้างโปรเจกต์
- `notebooks/03-app-prototype.ipynb`: Notebook สำหรับรันแอป Gradio (แนะนำสำหรับ demo)
- `notebooks/00-playground.ipynb`: Notebook ทดลองฟีเจอร์หลายส่วน
- `notebooks/01-text-pre-processing.ipynb`: Notebook สำหรับ preprocessing/summary แบบ batch
- `notebooks/nb01-np-text-masking.ipynb`: Notebook เวอร์ชัน batch เพิ่มเติม
- `.env.example`: ตัวอย่าง environment variables
- `data/raw/`: วางไฟล์ข้อมูลนำเข้า (ต้องสร้างเองหลัง clone)
- `data/processed/`: ผลลัพธ์ที่ประมวลผลแล้ว (ต้องสร้างเองหลัง clone)

## สิ่งที่ต้องมี
- Python 3.11+
- Git
- OpenAI API Key
- อินเทอร์เน็ต (สำหรับดาวน์โหลดโมเดลครั้งแรกจาก Hugging Face)

## ขั้นตอนติดตั้งและเริ่มใช้งาน
1. Clone โปรเจกต์
```bash
git clone https://github.com/master0slave/poc-privacy-preserving-voc-data.git
cd poc-privacy-preserving-voc-data
```

2. ติดตั้ง `uv` (ถ้ายังไม่มี)
```bash
pip install uv
```

3. ติดตั้ง dependency ด้วย `uv`
```bash
uv sync
```

4. ตั้งค่า environment variables
```bash
cp .env.example .env
```
เปิดไฟล์ `.env` แล้วใส่ค่า OpenAI key:
```env
OPENAI_API_KEY=your_openai_api_key_here
```

5. สร้างโฟลเดอร์ข้อมูล (ถ้ายังไม่มี)
```bash
mkdir -p data/raw data/processed
```

## วิธีรันโปรเจกต์

### โหมดแอป (แนะนำ)
1. เปิด Jupyter Lab
```bash
uv run jupyter lab
```
2. เปิดไฟล์ `notebooks/03-app-prototype.ipynb`
3. กด Run All Cells
4. เมื่อรันถึง `app.launch()` จะมีลิงก์ local (เช่น `http://127.0.0.1:7860`) ให้เปิดใช้งาน

### โหมด batch กับไฟล์ CSV
1. วางไฟล์ข้อมูลใน `data/raw/`
2. ใช้ไฟล์ notebook ตามเคส
- `notebooks/01-text-pre-processing.ipynb` คาดหวังไฟล์ `data/raw/voc-data-202401-202404.csv`
- `notebooks/nb01-np-text-masking.ipynb` คาดหวังไฟล์ `data/raw/voc-data.csv`
3. คอลัมน์สำคัญที่ต้องมีใน CSV
- `หมายเลขเสียง`
- `รายละเอียดเสียง`
4. รัน Notebook แบบ Run All
5. ผลลัพธ์จะถูกบันทึกใน `data/processed/` เป็นไฟล์ `.csv`/`.xlsx`

## หมายเหตุสำคัญ
- ห้าม hardcode API key ในโค้ดหรือ notebook
- ไฟล์ `.env` ถูกตั้งค่าใน `.gitignore` แล้ว แต่ให้ตรวจสอบทุกครั้งก่อน `git add .`
- หากเปิดโปรเจกต์ครั้งแรกแล้วช้า เป็นปกติจากการดาวน์โหลดโมเดลและ dependency

## Troubleshooting
- `OPENAI_API_KEY` ไม่ถูกต้อง: ตรวจสอบค่าใน `.env` และเปิด notebook ใหม่แล้ว Run All อีกครั้ง
- รันแล้ว import ไม่เจอโมดูล: รัน `uv sync` ซ้ำ
- Gradio ไม่ขึ้น URL: ตรวจสอบว่า cell `app.launch()` รันสำเร็จและไม่มี error ใน cell ก่อนหน้า
