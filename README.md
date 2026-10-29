
# Module 1: Căn cứ số (Terminal)

Một dòng lệnh tạo cây thư mục:

```bat
mkdir greentech-system\docs greentech-system\scripts greentech-system\data greentech-system\backup
```

Di chuyển bằng đường dẫn tương đối và tạo `.env` ẩn:

```bat
cd greentech-system\scripts
cd ..
echo APP_ENV=demo>.env
echo DB_PASSWORD=demo_password_gia>>.env
attrib +h .env
```

Lưu ý: tên thư mục không dấu, không khoảng trắng; không để khoảng trắng trước dấu `>`; mật khẩu chỉ là giá trị giả.

# Module 2: Robot lọc log (`scripts/auto_report.bat`)

```bat
@echo off
setlocal
for %%I in ("%~dp0..") do set "ROOT=%%~fI"
set "LOG=%ROOT%\data\log-he-thong.log"
set "OUT=%ROOT%\backup\bao-cao-su-co.txt"
set "FILTERED=%TEMP%\greentech_filtered.txt"

if not exist "%LOG%" (
    echo [LOI] Khong tim thay file log
    exit /b 1
)

>>"%OUT%" echo ============================================
>>"%OUT%" echo   BAO CAO SU CO HE THONG GREENTECH
>>"%OUT%" echo   Thoi gian tao: %date% %time%
>>"%OUT%" echo ============================================

type "%LOG%" | findstr /C:"CRITICAL" /C:"ERROR" /C:"TIMEOUT" > "%FILTERED%"
type "%FILTERED%" >> "%OUT%"
del "%FILTERED%"

echo Hoan tat: %OUT%
endlocal
exit /b 0
```

- Pipe `|` truyền từng dòng nên file 500MB không làm tràn RAM.
- `>>` nối tiếp vào báo cáo, không ghi đè.
- Audit: không dùng `del /f /s /q`. Lệnh `del "%FILTERED%"` chấp nhận được vì chỉ xóa đúng một file tạm, không wildcard.

# Module 3: Tài liệu

**README.md** cần đủ 4 phần:

- H1: `# GreenTech System`
- Code Block: lệnh chạy `auto_report.bat`
- Task List: `- [x]` và `- [ ]`
- Table: bảng mã linh kiện, tên thiết bị, loại lưu trữ

**SRS (Word)**, làm theo 3 bước:

1. Gán Heading 1, 2, 3 trong Home, nhóm Styles.
2. Ở trang đầu: References, Table of Contents, Automatic Table.
3. Sau khi sửa nội dung: Update Field (F9).

# Module 4: Excel

Tại ô E2 của sheet `trang-thai-tram`, rồi kéo xuống:

```excel
=VLOOKUP(TRIM(B2), 'danh-muc-linh-kien'!$A$2:$D$101, 2, FALSE)
```

- `TRIM` xóa khoảng trắng thừa gây `#N/A`.
- `$` khóa vùng tra cứu khi kéo công thức.
- `FALSE` bắt buộc khớp chính xác.
- `2` lấy cột `ten_thiet_bi`.
- Giữ vùng dữ liệu không có hàng trống hay Merge Cell để AutoFilter hoạt động.

# Bẫy Hardware

`MB/s = Mbps / 8`, rồi `Thời gian = Dung lượng / MB/s`. Với file 500MB: 100 Mbps cho 12,5 MB/s, tức 40 giây.
