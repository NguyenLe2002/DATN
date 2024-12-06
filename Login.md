# Login.html

$$
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Đăng nhập</title>
  <link rel="stylesheet" href="css/index.css">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>

  <div class="container">

    <!-- Phần đầu trang -->
    <header class="section header">
        <div class="logo">
          <img src="images/logo_school.png" alt="Logo">
        </div>
        <div class="school-name">
            <div class="school-info">
              <p class="blue_1">HỌC VIỆN KỸ THUẬT MẬT MÃ</p>
              <p class="blue_1">KHOA KỸ THUẬT ĐIỆN TỬ VIỄN THÔNG</p>
              <p class="blue_2">ĐỒ ÁN TỐT NGHIỆP</p>
              <p class="red">THIẾT KẾ HỆ THỐNG GIÁM SÁT ĐIỀU KIỆN TỰ NHIÊN VÀ</p>
              <p class="red">DINH DƯỠNG CỦA VƯỜN RAU THUỶ CANH</p>
              <p class="green">GVHD: ThS. PHAN THỊ THANH HUYỀN - SVTH: NGUYỄN HỮU LỄ - MSV: DT040131</p>
            </div>
      </header>

    <!-- Phần thân trang -->
    <div class="content_1">
        <div class="login-container">
            <h2>Đăng Nhập</h2>
            <form id="loginForm">
                <input type="text" id="username" name="username" placeholder="Tên đăng nhập" required>
                <input type="password" id="password" name="password" placeholder="Mật khẩu" required>
                <button type="submit">Đăng Nhập</button>
                <div id="errorMessage" class="error-message"></div>
            </form>
        </div>
    </div>
  </div>
  <script>

    document.getElementById('loginForm').addEventListener('submit', async function(event) {
        event.preventDefault(); // Ngăn chặn hành vi mặc định của form

        const username = document.getElementById('username').value;
        const password = document.getElementById('password').value;
        const errorMessage = document.getElementById('errorMessage');

        try {
            // Gửi yêu cầu đăng nhập đến server
            const response = await fetch('http://localhost:3001/login', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ username, password }) // Gửi dữ liệu dưới dạng JSON
            });

            const result = await response.json();

            if (result.success) {
                window.location.href = 'index.html'; // Đăng nhập thành công
            } else {
                errorMessage.textContent = 'Tài khoản hoặc mật khẩu không đúng'; // Lỗi đăng nhập
            }
        } catch (error) {
            errorMessage.textContent = 'Có lỗi xảy ra. Vui lòng thử lại sau.'; // Lỗi kết nối
        }
    });
    </script>

</body>
</html>
$$





