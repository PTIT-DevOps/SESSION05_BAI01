# BT01 - Khôi phục commit đã mất bằng Git Reflog (Reflog Recovery)

## 1. Khởi tạo repository và tạo commit

```powershell
PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git init
Initialized empty Git repository in D:/Work/Rikkei/devops/BT/SS05/SS05-BT01/.git/
PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git config --local user.name "lethinh"
PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git config --local user.email "lephuthinh387@gmail.com"

PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git add .
PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git commit -m "khoi tao du an"
[main (root-commit) 9f8e7d6] khoi tao du an
 1 file changed, 1 insertion(+)
 create mode 100644 app.js

PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> Set-Content -Path feature.txt -Value "Day la tinh nang quan trong"
PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git add .
PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git commit -m "Them tinh nang quan trong"
[main e3a5b2c] Them tinh nang quan trong
 1 file changed, 1 insertion(+)
 create mode 100644 feature.txt
```

## 2. Giả lập lỗi làm mất commit

```powershell
PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git reset --hard HEAD~1
HEAD is now at 9f8e7d6 khoi tao du an

PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git log --oneline
9f8e7d6 (HEAD -> main) khoi tao du an

PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> Test-Path feature.txt
False
```

Commit `e3a5b2c` không còn xuất hiện trên `git log`, tệp `feature.txt` cũng bị xóa khỏi thư mục làm việc.

## 3. Tra cứu Reflog để tìm lại commit

```powershell
PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git reflog
9f8e7d6 (HEAD -> main) HEAD@{0}: reset: moving to HEAD~1
e3a5b2c HEAD@{1}: commit: Them tinh nang quan trong
9f8e7d6 HEAD@{2}: commit (initial): khoi tao du an
```

Dòng `HEAD@{1}` là commit vừa bị mất, mã hash cần khôi phục là `e3a5b2c`.

## 4. Khôi phục commit về nhánh hiện tại

```powershell
PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git reset --hard e3a5b2c
HEAD is now at e3a5b2c Them tinh nang quan trong
```

## 5. Kiểm tra kết quả

```powershell
PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git log --oneline
e3a5b2c (HEAD -> main) Them tinh nang quan trong
9f8e7d6 khoi tao du an

PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> Get-Content feature.txt
Day la tinh nang quan trong

PS D:\Work\Rikkei\devops\BT\SS05\SS05-BT01> git reflog
e3a5b2c (HEAD -> main) HEAD@{0}: reset: moving to e3a5b2c
9f8e7d6 HEAD@{1}: reset: moving to HEAD~1
e3a5b2c HEAD@{2}: commit: Them tinh nang quan trong
9f8e7d6 HEAD@{3}: commit (initial): khoi tao du an
```
