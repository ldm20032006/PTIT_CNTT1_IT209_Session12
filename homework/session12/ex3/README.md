# Bài 3: Tách biệt luồng CI/CD đa môi trường với GitHub Environments

## 1. Mục tiêu

- Tạo và quản lý hai môi trường Staging và Production bằng GitHub Environments.
- Xây dựng workflow CD tự động theo từng nhánh Git.
- Cấu hình cơ chế phê duyệt thủ công trước khi triển khai Production.
- Kiểm tra quá trình thực thi workflow trên GitHub Actions.

## 2. Công nghệ sử dụng

- Git và GitHub
- GitHub Actions
- GitHub Environments
- YAML

## 3. Cấu hình GitHub Environments

Truy cập **Repository → Settings → Environments**.

Tạo hai môi trường:

- **staging:** Môi trường kiểm thử, không yêu cầu phê duyệt.
- **production:** Môi trường triển khai chính thức, yêu cầu phê duyệt thủ công thông qua Required reviewers.

Đối với Production, bật Required reviewers và chọn tài khoản GitHub có quyền phê duyệt.

## 4. Cấu hình workflow

Tạo file `.github/workflows/cd-multi-env.yml`.

Workflow được kích hoạt khi có sự kiện push vào hai nhánh:

- `develop`: Thực thi Job `deploy-staging`.
- `main`: Thực thi Job `deploy-prod`.

### Job deploy-staging

- Chạy trên `ubuntu-latest`.
- Sử dụng environment `staging`.
- Tự động thực thi khi push vào nhánh `develop`.
- Mô phỏng quá trình triển khai bằng lệnh `echo`.

### Job deploy-prod

- Chạy trên `ubuntu-latest`.
- Sử dụng environment `production`.
- Kích hoạt khi push vào nhánh `main`.
- Chờ người có quyền phê duyệt trước khi thực thi các bước triển khai.

## 5. Kiểm tra hoạt động

**Trường hợp 1: Push vào develop**

- GitHub Actions tự động kích hoạt workflow.
- Job `deploy-staging` được thực thi.
- Workflow hoàn thành thành công và hiển thị dấu tích xanh.

**Trường hợp 2: Push vào main**

- GitHub Actions kích hoạt workflow.
- Job `deploy-prod` chuyển sang trạng thái Waiting for review.
- Người có quyền phê duyệt chọn Review deployments.
- Chọn môi trường production và nhấn Approve and deploy.
- Job tiếp tục thực thi và hoàn thành thành công.

## 6. Kết quả

- Đã cấu hình hai môi trường Staging và Production.
- Đã xây dựng workflow CD phân biệt theo nhánh Git.
- Môi trường Staging được triển khai tự động.
- Môi trường Production được bảo vệ bằng cơ chế phê duyệt thủ công.
- Kiểm tra thành công quy trình mô phỏng triển khai đa môi trường.

