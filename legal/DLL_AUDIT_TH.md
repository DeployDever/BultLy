> รายงานประวัติของ EXE ก่อนปรับเป็น Runtime ภายนอก: ชุดซอร์สใหม่ไม่นำ
> Visual C++ DLL 6 ตัวในรายงานนี้ไปแจก ดู EXTERNAL_RUNTIME_TH.md และ
> legal/current-build/NATIVE_REVIEW.json หลัง build สำหรับผลรุ่นใหม่

# ผลตรวจ DLL ของ BultLy — 26 กันยายน 2026

ตรวจจาก BultLy.exe ภายใน BultLy-Windows(1).zip ที่ผู้พัฒนาส่งมา
SHA-256 ของ EXE: 08e71423cd4b0c1bf421d202751accbd32d5285df75b3c41cfc451c9e81bee5f

ตรวจ DLL 121 รายการ โดยอ่าน archive ของ EXE และเทียบ SHA-256 กับสมาชิกในแพ็กเกจ
ที่ดาวน์โหลดจากช่องทางทางการ ไม่ได้รัน EXE หรือ DLL บน Windows

| กลุ่ม | จำนวน DLL | แหล่งที่พบไฟล์ตรงกัน |
| --- | ---: | --- |
| Microsoft facade assemblies ใน pythonnet/runtime | 96 | Microsoft.NET.Build.Extensions 2.2.101 จาก NuGet |
| Python และ runtime ที่มากับ Python | 6 | Python 3.13.14 embeddable x64 จาก python.org |
| PortAudio | 3 | sounddevice 0.5.6 wheel จาก PyPI |
| LLVM และ MSVCP ที่มากับ llvmlite | 2 | llvmlite 0.47.0 wheel จาก PyPI |
| OpenBLAS และ MSVCP ที่มากับ NumPy | 2 | NumPy 2.3.5 wheel จาก PyPI |
| ClrLoader | 2 | clr_loader 0.3.1 wheel จาก PyPI |
| Python.Runtime | 1 | pythonnet 3.1.0 wheel จาก PyPI |
| WebView2 และ WebBrowserInterop | 7 | pywebview 6.2.1 wheel จาก PyPI; คงเงื่อนไข WebView2 แยก |
| MSVCP140 และ VCOMP140 ที่ระดับราก | 2 | Visual C++ Redistributable 14.51.36247 จาก Microsoft |

ผล: 121/121 มี upstream hash match; 117 รายการมีเอกสารอ้างอิงประกอบ
และอีก 4 รายการยังต้องยืนยันสิทธิ์แจกจ่ายของผู้จำหน่าย
การรู้แหล่งที่มาไม่เท่ากับสิทธิ์แจกจ่ายโดยไม่มีเงื่อนไข

## ข้อค้นพบที่แก้แล้ว

- ตัวตรวจเดิมติดป้ายรอตรวจ 114 รายการเพราะมีฐานอ้างอิงเพียง 7 รายการ
- System.* จำนวน 96 รายการไม่ใช่ส่วนที่ใช้ MIT ของ Python.NET:
  แพ็กเกจทางการระบุ Microsoft .NET Library license ผ่าน licenseUrl ใน nuspec
  ได้แนบข้อความฉบับเต็มและ HTML ต้นทางไว้แล้ว
- DLL ของ Python, libffi, OpenSSL, PortAudio, LLVM, OpenBLAS และตัวเชื่อมต่าง ๆ
  มีหลักฐานเทียบไฟล์จริง ไม่ได้ใช้แค่ชื่อไฟล์หรือ metadata ของแพ็กเกจ
- BINARY_LICENSE_REFERENCES.json บันทึกแฮช DLL, URL/แฮช archive, สมาชิกต้นทาง,
  เอกสาร และแฮชเอกสาร ส่วน DLL_UPSTREAM_PROVENANCE.json บันทึกวิธีตรวจ
- release_support.py ตรวจ DLL ทั้งใน binaries และ datas และตรวจแฮชเอกสาร
- verify_release.py ตรวจว่า DLL ทุกตัวใน EXE มีรายการตรวจ ไม่มีรายการซ้ำ
  ไบต์ DLL ตรงกับรายงาน และเอกสารที่อ้างถึงถูกฝังและไม่เปลี่ยนแปลง
- ไฟล์ .py/.pyc ในโฟลเดอร์ที่ชื่อ licenses จะไม่ถูกเก็บเป็นเอกสารใบอนุญาตอีก
- APP_LICENSE.txt กรอกข้อมูลแล้ว ให้สิทธิ์ผู้ซื้อหนึ่งคนใช้หลายเครื่องของตนเอง
  และอ้างเงื่อนไข Microsoft .NET Library ฉบับเต็มไว้

## เรื่องที่ต้องยืนยันก่อนขาย: Visual C++ Runtime 4 รายการ

1. MSVCP140.dll — 14.51.36247.0
2. VCOMP140.DLL — 14.51.36247.0
3. llvmlite.libs/msvcp140-8f141b4454fa78db34bc1f28c571b4da.dll — 14.44.35208.0
4. numpy.libs/msvcp140-a4c2229bdc2a2a630acdc095b4d86008.dll — 14.40.33810.0

ข้อ 1–2 ตรงกับ installer ทางการ ส่วนข้อ 3–4 ตรงกับ wheels ทางการ
แต่ใบอนุญาต BSD ของ NumPy/llvmlite ไม่ใช่หลักฐานสิทธิ์แจก Microsoft runtime
และใบอนุญาตสำหรับติดตั้ง Visual C++ Runtime อย่างเดียวไม่ให้สิทธิ์แจกต่อ
ต้องมีสิทธิ์ตาม Visual Studio/Build Tools ที่ใช้ได้กับการแจกนั้น หรือสิทธิ์อื่น
จาก Microsoft พร้อม REDIST list ที่เกี่ยวข้อง จึงคง review_required ไว้จริง
ไม่ปรับเป็นผ่านเพื่อให้ตัวเลขเหลือศูนย์

อ้างอิง: https://learn.microsoft.com/en-us/cpp/windows/redistributing-visual-cpp-files
ใบอนุญาต Runtime ที่ installer อ้างถึง: https://aka.ms/VCRedistLicense

VCRUNTIME140.dll และ VCRUNTIME140_1.dll ในชุดนี้ตรงกับ Python embeddable ทางการ
ได้อ้างใบอนุญาต Windows binary distribution ของ Python ฉบับตรงกับรุ่นไว้
ไม่ได้ใช้ข้อสรุปนี้กับ MSVCP/VCOMP อีก 4 รายการข้างต้น

## วิธีใช้ชุดแก้

แตก BultLy-license-dll-fix.zip ทับโฟลเดอร์ซอร์สที่มี Build_EXE.bat โดยคงโครงสร้าง
legal และ tests จากนั้นเปิด Build_EXE.bat เพื่อสร้าง EXE ใหม่
ชุดแก้ไม่ใช่ Windows EXE ที่คอมไพล์ใหม่ และการแก้ไฟล์ข้าง EXE เก่าอย่างเดียว
จะไม่เปลี่ยนเอกสารหรือรายงานที่ถูกฝังอยู่ใน EXE เก่า

หากใช้ DLL เดิมทั้งหมด รายงานใหม่ควรเป็น 121 รายการ ตรวจต่อ 4 รายการ
ถ้ามี DLL เปลี่ยนรุ่นหรือแฮช ต้องตรวจรุ่นนั้นใหม่ ไม่ใช้ผลนี้แทน

ให้ลูกค้าอ่านและยอมรับ APP_LICENSE.txt และเงื่อนไข Microsoft ที่อ้างถึง
ก่อนซื้อหรือเริ่มใช้งาน เช่น ช่องยอมรับข้อตกลงในขั้นตอนชำระเงิน
ชุดแก้นี้ไม่ได้เพิ่มหน้าต่างยอมรับข้อตกลงในแอป

ทดสอบตัวช่วย packaging/audit ผ่าน 11 tests รวมกรณี DLL/เอกสารถูกเปลี่ยน
และ DLL ไม่มีรายการตรวจ ผลนี้ไม่แทนการทดสอบเสียง/หน้าต่างบน Windows
เอกสารนี้ตรวจสิทธิ์ DLL ตามหลักฐานข้างต้น ไม่รับรองชื่อ โลโก้ หรือสิทธิ์ทุกด้านของผลิตภัณฑ์
