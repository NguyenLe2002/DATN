# CODE SERVER

$$
const express = require('express');
const bodyParser = require('body-parser');
const mysql = require('mysql2');
const cors = require('cors'); // Import cors

const app = express();
const port = 3001;

// Middleware để parse body dưới dạng JSON
app.use(express.json());

app.use(cors());

app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

// Kết nối MySQL
const db = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: '123456789',
    database: 'do_an_tot_nghiep'
});

db.connect((err) => {
    if (err) {
        console.error('Không thể kết nối đến MySQL:', err);
        return;
    }
    console.log('Kết nối đến MySQL thành công!');
});

// Route xử lý đăng nhập
app.post('/login', (req, res) => {
    const { username, password } = req.body;
    // In ra giá trị của username và password
    console.log('Username:', username);
    console.log('Password:', password);

    // Truy vấn cơ sở dữ liệu để kiểm tra tài khoản và mật khẩu
    const query = 'SELECT * FROM user WHERE username = ? AND password = ?';
    db.query(query, [username, password], (err, results) => {
        if (err) {
            res.status(500).json({ success: false, message: 'Lỗi máy chủ' });
            return;
        }

        // Kiểm tra nếu kết quả trả về có ít nhất 1 dòng
        if (results.length > 0) {
            res.json({ success: true });  // Đăng nhập thành công
        } else {
            res.json({ success: false }); // Tài khoản hoặc mật khẩu không đúng
        }
    });
});

// Router xử lý thống kê
app.get('/api/search', (req, res) => {
    let { fromDate, toDate, criteria } = req.query;

    // Ánh xạ tiêu chí đến các bảng khác nhau
    let table;
    if (criteria === 'temperature') {
        table = 'nhiet_do';
    } else if (criteria === 'humidity') {
        table = 'do_am';
    } else if (criteria === 'ph') {
        table = 'do_tds';
    } else if (criteria === 'light') {
        table = 'anh_sang';
    } else if (criteria === 'solutionTemp') {
        table = 'nhiet_do_dung_dich';
    } else {
        return res.status(400).send('Invalid criteria');
    }

    // Tạo câu truy vấn với bảng động
    let query = `SELECT thoi_gian, gia_tri FROM ${table} WHERE thoi_gian BETWEEN ? AND ?`;
    fromDate = `${fromDate} 00:00:00`;
    toDate = `${toDate} 23:59:59`;
    // Thực thi truy vấn với các giá trị từ ngày và đến ngày
    db.query(query, [fromDate, toDate], (err, results) => {
        if (err) {
            return res.status(500).send('Error retrieving data');
        }
        res.json(results);
    });
});

app.get('/api/LastData', (req, res) => {
    const { table } = req.query;
    if (!table) {
        return res.status(400).send('Tên bảng không hợp lệ');
    }
    // Truy vấn 10 điểm cuối cùng từ bảng
    const query = `SELECT * FROM ${table} ORDER BY id DESC LIMIT 10`;
    console.log(table)
    db.query(query, (err, result) => {
        if (err) {
            return res.status(500).send(err);
        }
        console.log(result)
        res.json(result);
    });
});

let temperature=0, humidity=0, ph=0, light=0, tempC=0;

//Route nhận dữ liệu từ ESP8266
app.post('/endpoint', (req, res) => {
    const sensorData = req.body;
    console.log(sensorData);
    temperature = sensorData.Temperature; 
    humidity = sensorData.Humidity;
    ph = sensorData.TDS; 
    light = sensorData.Light;
    tempC = sensorData.TempC;

    // Kiểm tra nếu tất cả giá trị đều hợp lệ
    if (temperature !== undefined && humidity !== undefined && ph !== undefined && light !== undefined) {
        // Thực hiện lưu dữ liệu vào các bảng tương ứng
        const insertTemperature = `INSERT INTO nhiet_do (gia_tri) VALUES (?)`;
        db.query(insertTemperature, [temperature], (err, result) => {
            if (err) {
                console.error('Lỗi khi lưu dữ liệu nhiệt độ:', err);
                return res.status(500).send('Lỗi khi lưu dữ liệu nhiệt độ');
            }
            console.log('Dữ liệu nhiệt độ đã được lưu:', result);
        });

        const insertHumidity = `INSERT INTO do_am (gia_tri) VALUES (?)`;
        db.query(insertHumidity, [humidity], (err, result) => {
            if (err) {
                console.error('Lỗi khi lưu dữ liệu độ ẩm:', err);
                return res.status(500).send('Lỗi khi lưu dữ liệu độ ẩm');
            }
            console.log('Dữ liệu độ ẩm đã được lưu:', result);
        });

        const insertPh = `INSERT INTO do_tds (gia_tri) VALUES (?)`;
        db.query(insertPh, [ph], (err, result) => {
            if (err) {
                console.error('Lỗi khi lưu dữ liệu độ pH:', err);
                return res.status(500).send('Lỗi khi lưu dữ liệu độ pH');
            }
            console.log('Dữ liệu độ pH đã được lưu:', result);
        });

        const insertLight = `INSERT INTO anh_sang (gia_tri) VALUES (?)`;
        db.query(insertLight, [light], (err, result) => {
            if (err) {
                console.error('Lỗi khi lưu dữ liệu ánh sáng:', err);
                return res.status(500).send('Lỗi khi lưu dữ liệu ánh sáng');
            }
            console.log('Dữ liệu ánh sáng đã được lưu:', result);
        });

        const insertTempC = `INSERT INTO nhiet_do_dung_dich (gia_tri) VALUES (?)`;
        db.query(insertTempC, [tempC], (err, result) => {
            if (err) {
                console.error('Lỗi khi lưu dữ liệu ánh sáng:', err);
                return res.status(500).send('Lỗi khi lưu dữ liệu ánh sáng');
            }
            console.log('Dữ liệu ánh sáng đã được lưu:', result);
        });

        // Gửi phản hồi thành công
        res.send('Dữ liệu đã được nhận và lưu thành công!');
    } else {
        console.log('Không nhận được dữ liệu hoặc dữ liệu bị thiếu.');
        res.status(400).send('Không nhận được dữ liệu hoặc dữ liệu bị thiếu.');
    }
});

app.get('/api/sensors', (req, res) => {
    // Dữ liệu bạn đã phân tách và lưu vào biến
    const sensorData = {
        temperature: temperature,
        humidity: humidity,
        ph: ph,
        light: light,
        tempC: tempC
    };
    // Gửi dữ liệu dưới dạng JSON
    res.json(sensorData);
});

// API để nhận lệnh điều khiển từ web và chuyển xuống thiết bị
let controlData = {};
app.post('/api/control', async (req, res) => {
    controlData = req.body;
    console.log ('Dữ liệu gửi esp: ', controlData);
    res.json({ message: 'Dữ liệu đã được nhận', data: controlData });
});
app.get('/control', (req, res) => {
    // Gửi dữ liệu dưới dạng JSON
    res.json(controlData);
});

// Khởi động server
app.listen(port, () => {
    console.log(`Server đang chạy trên cổng ${port}`);
});
$$





