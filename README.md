# Bai 1 - Khoi phuc commit da mat bang Git Reflog

## 1. Muc tieu
- Hieu cach Git luu lich su hoat dong cuc bo qua Reflog.
- Dung `git reflog` de truy vet commit cu.
- Khoi phuc commit bi mat sau `git reset --hard HEAD~1` ma khong viet lai code thu cong.

## 2. Moi truong thuc hanh
- Duong dan demo: `ss5/bai1/demo-reflog/`
- Git version: `2.49.0.windows.1`
- Cau hinh:
  ```
  git init -b master
  git config user.name "LeeAnhs06"
  git config user.email "vanle9540@gmail.com""
  ```
- Luu y: lam trong thu muc demo rieng, khong anh huong code Java o `bai1/`.
- Tren Git Bash dung `cat`, khong dung `type` (lenh `type` chi co tren `cmd`). Tren `cmd` dung `type`, `dir`.

## 3. Buoc 1 - Tao 2 commit (bat buoc de `HEAD~1` co nghia)

```bash
echo khoi tao > init.txt
git add init.txt
git commit -m "Init commit"

echo Day la tinh nang quan trong > feature.txt
git add feature.txt
git commit -m "Them tinh nang quan trong"

git log --oneline
```

Ket qua that:
```
a3c10b1 Them tinh nang quan trong
dac3942 Init commit
```

> Giai thich: can commit nen `Init commit` truoc. Neu chi co 1 commit ma chay
> `git reset --hard HEAD~1` se loi:
> `fatal: ambiguous argument 'HEAD~1': unknown revision...`

## 4. Buoc 2 - Gia lap loi mat commit

```bash
git reset --hard HEAD~1
git log --oneline
ls
cat feature.txt
```

Ket qua mong doi:
- `git log --oneline` chi con:
  ```
  dac3942 Init commit
  ```
- `feature.txt` bien mat, `cat feature.txt` bao `No such file`.
- Day la luc phat hien mat commit quan trong.

## 5. Buoc 3 - Tra cuu Reflog

```bash
git reflog
```

Vi du ket qua:
```
a3c10b1 HEAD@{0}: reset: moving to HEAD~1
a3c10b1 HEAD@{1}: commit: Them tinh nang quan trong
dac3942 HEAD@{2}: commit (initial): Init commit
```

Giai thich:
- `git log` chi hien lich su cua nhanh hien tai -> commit bi reset se bien mat.
- `git reflog` luu moi lan di chuyen cua HEAD (khoang 90 ngay) -> van thay hash cu.
- Cot dau tien (`a3c10b1`, `dac3942`) la short hash (7 ky tu). Xem full hash bang:
  ```
  git rev-parse HEAD
  git log --format=%H -1
  ```
- Trong bai lam truoc hash tim duoc la `c107122` voi dong:
  ```
  c107122 HEAD@{0}: commit (initial): Them tinh nang quan trong
  ```
  Do chinh la hash can dung de khoi phuc.

## 6. Buoc 4 - Khoi phuc (khong viet lai code tay)

Cach chinh:
```bash
git reset --hard a3c10b1
# thay a3c10b1 bang hash that tim duoc o Buoc 3
# hoac dung nhanh: git reset --hard HEAD@{1}
```

Cach du phong (tuong duong):
```bash
git checkout -b recovery a3c10b1
git cherry-pick a3c10b1
```

## 7. Buoc 5 - Kiem chung

```bash
git log --oneline
cat feature.txt
git status
```

Ket qua mong doi:
```
a3c10b1 Them tinh nang quan trong
dac3942 Init commit
```
- `cat feature.txt` ra: `Day la tinh nang quan trong`
- `git status`: `nothing to commit, working tree clean`
- Khop voi lich su truoc khi bi reset -> khoi phuc thanh cong.

## 8. Ket luan
- Da khoi phuc commit chi bang co che Reflog, dung rang buoc khong viet lai code.
- Bai hoc: truoc khi `reset --hard` luon kiem tra `git log`, `git status`; neu lo tay thi dung `git reflog` ngay, khong tao commit moi de tranh lam troi reflog.
