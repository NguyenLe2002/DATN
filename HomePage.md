# homePage.js

$$
document.addEventListener('DOMContentLoaded', function() {
    const buttons = document.querySelectorAll('.sidebar button');
    const mainContent = document.getElementById('mainContent');

    // Hàm để thiết lập sự kiện cho các nút trong phần biểu đồ
    function setupChartControls() {
        document.getElementById('tempButton').addEventListener('click', () => updateChartData('Nhiệt độ', '°C'));
        document.getElementById('humidityButton').addEventListener('click', () => updateChartData('Độ ẩm', '%'));
        document.getElementById('lightButton').addEventListener('click', () => updateChartData('Ánh sáng', 'Lux'));
        document.getElementById('solutionTempButton').addEventListener('click', () => updateChartData('Nhiệt độ dung dịch', '°C'));
        document.getElementById('phButton').addEventListener('click', () => updateChartData('Độ pH', 'ppm'));
    }

    function updateChartData(type, unit) {
    if (typeof lineChart === 'undefined') {
        console.error('lineChart is not defined');
        return;
    }
    lineChart.data.datasets[0].label = `${currentDataType} (${currentUnit})`;
    lineChart.options.scales.y.title.text = `Value (${currentUnit})`;
    lineChart.update();
    
    // Cập nhật màu nút bấm
    document.querySelectorAll('.chart-controls button').forEach(btn => btn.classList.remove('active'));
    document.getElementById(`${type.toLowerCase()}Button`).classList.add('active');
}

    // Thiết lập sự kiện click cho từng nút bấm
    buttons.forEach(button => {
        
        button.addEventListener('click', function() {

            // Thay đổi nội dung của mainContent dựa vào nút được click
            switch (button.id) {
                case 'btn-home':
                    buttons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                    mainContent.innerHTML = `
                        <br>
                        <p>
                            Trong bối cảnh hiện nay, khi nhu cầu về thực phẩm sạch và an toàn ngày càng tăng cao, công nghệ thủy canh đã trở thành một giải pháp tiên tiến giúp nâng cao hiệu quả sản xuất nông nghiệp và giảm thiểu tác động tiêu cực đến môi trường. Tuy nhiên, để đảm bảo sự phát triển ổn định và tối ưu hóa chất lượng của cây trồng trong hệ thống thủy canh, việc giám sát chặt chẽ các điều kiện tự nhiên như nhiệt độ, độ ẩm, ánh sáng, cũng như các yếu tố dinh dưỡng trong nước là vô cùng cần thiết. Các phương pháp truyền thống thường yêu cầu sự can thiệp thủ công, dẫn đến việc quản lý không nhất quán và khó khăn trong việc tối ưu hóa quy trình trồng trọt.
                        </p>
                        <br>
                        <p>
                            Sự phát triển của công nghệ IoT (Internet of Things) đã mở ra cơ hội mới trong việc giám sát và quản lý các hệ thống trồng trọt một cách tự động và chính xác hơn. Việc áp dụng các cảm biến thông minh và hệ thống kết nối dữ liệu không chỉ giúp người trồng dễ dàng theo dõi các thông số quan trọng của vườn rau thủy canh theo thời gian thực mà còn hỗ trợ đưa ra các quyết định kịp thời, góp phần nâng cao năng suất và chất lượng cây trồng.
                        </p>
                        <br>
                        <p>
                            Xuất phát từ nhu cầu thực tế này, tôi đã chọn đề tài "Nghiên cứu thiết kế hệ thống giám sát điều kiện tự nhiên và dinh dưỡng của vườn rau thủy canh" để làm đồ án tốt nghiệp. Đề tài hướng đến việc thiết kế và xây dựng một hệ thống giám sát tự động, có khả năng theo dõi và điều chỉnh các điều kiện tự nhiên và dinh dưỡng của vườn rau thủy canh, nhằm tối ưu hóa quy trình trồng trọt và góp phần vào việc phát triển nông nghiệp bền vững.
                        </p>
                    `;

                    break;
                case 'btn-control':
                    buttons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                    mainContent.innerHTML = `
                        <div class="main-content_1">
                            <div>
                                <div style="margin-top: 110px; margin-left: 100px; font-weight: bold">
                                    <h1 style="color: blue; font-size: 25px;">Thông số không khí</h1>
                                    <p style="color: black; font-size: 20px;">Nhiệt độ: <span id="temperature1">N/A</span> °C</p>
                                    <p style="color: black; font-size: 20px;">Độ ẩm: <span id="humidity1">N/A</span> %</p>
                                    <p style="color: black; font-size: 20px;">Ánh sáng: <span id="light1">N/A</span> Lx</p>
                                </div>
                            </div>
                            <div class="control-middle">
                                <h1 style="color: blue; font-size: 30px; text-align: center">Chọn chế độ điều khiển</h1>
                                <br>
                                <div style="text-align: center; font-size: 14px">
                                    <label>
                                        <input type="radio" name="controlMode" id="autoMode" checked> AUTO
                                    </label>
                                    <label style="margin-left: 10px">
                                        <input type="radio" name="controlMode" id="manualMode"> MANUAL
                                    </label>
                                </div>
                                <div style="margin-left: 140px; font-weight: bold; font-size: 20px">
                                    <h2 style="color: blue; font-size: 25px; margin-top: 20px">Bảng điều khiển</h2>
                                    <div style="margin-top: 10px">
                                        <input type="checkbox" id="fan">
                                        <label for="fan">Quạt</label>
                                    </div>
                                    <div style="margin-top: 5px">
                                        <input type="checkbox" id="light">
                                        <label for="light">Đèn</label>
                                    </div>
                                    <div style="margin-top: 5px">
                                        <input type="checkbox" id="roof">
                                        <label for="roof">Mái che</label>
                                    </div>
                                    <button class="btn-update" id="btn-update">Cập nhật</button>
                                </div>
                            </div>
                            <div>
                                <div style="margin-top: 110px; margin-left: 100px; font-weight: bold">
                                    <h1 style="color: blue; font-size: 25px;">Thông số dung dịch</h1>
                                    <p style="color: black; font-size: 20px;">Nhiệt độ: <span id="temperature2">N/A</span> °C</p>
                                    <p style="color: black; font-size: 20px;">TDS: <span id="tds2">N/A</span> ppm</p>
                                </div>
                            </div>
                        </div>
                    `;
                    
                    // Lấy các phần tử cần thiết
                    const autoMode = document.getElementById('autoMode');
                    const manualMode = document.getElementById('manualMode');
                    const checkboxes = document.querySelectorAll('#fan, #light, #roof');
                    const updateButton = document.getElementById('btn-update');

                    let currentMode = 'auto';  // Biến trạng thái để lưu trữ chế độ hiện tại

                    // Hàm để vô hiệu hóa hoặc kích hoạt các nút
                    function toggleControls(disabled) {
                        checkboxes.forEach(checkbox => {
                            checkbox.disabled = disabled;
                        });
                        updateButton.disabled = disabled;
                        if (disabled) {
                            updateButton.style.backgroundColor = 'gray';
                        } else {
                            updateButton.style.backgroundColor = 'red'; // Màu ban đầu của nút
                        }
                    }

                    // Định nghĩa các phần tử DOM
                    const temperatureElement = document.getElementById('temperature1');
                    const humidityElement = document.getElementById('humidity1');
                    const lightElement = document.getElementById('light1');
                    const tsdElement = document.getElementById('tds2');
                    const temperatureElement2 = document.getElementById('temperature2');

                    // Hàm để lấy dữ liệu từ server
                    const fetchData = async () => {
                        try {
                            const response = await fetch('http://localhost:3001/api/sensors'); // Địa chỉ endpoint trên server
                            const data = await response.json();

                            console.log('Dữ liệu nhận API', data);
                            
                            // Cập nhật giá trị hiển thị trên giao diện
                            temperatureElement.innerText = data.temperature !== undefined ? data.temperature : 'Not';
                            humidityElement.innerText = data.humidity !== undefined ? data.humidity : 'N/Not';
                            lightElement.innerText = data.light !== undefined ? data.light : 'Not/A';
                            tsdElement.innerText = data.ph !== undefined ? data.ph : 'Not/A';
                            temperatureElement2.innerText = data.tempC !== undefined ? data.tempC : 'Not';

                        } catch (error) {
                            console.error('Error fetching data:', error);
                            temperatureElement.innerText = 'Not';
                            humidityElement.innerText = 'Not';
                            lightElement.innerText = 'Not';
                            tsdElement.innerText = 'Not';
                            temperatureElement2.innerText = 'Not';
                        }
                    };

                    // Hàm gửi dữ liệu khi chọn chế độ AUTO
                    const sendAutoModeData = async () => {
                        const data = { 
                            mode: 'auto',
                            fan: 0,
                            light: 0,
                            roof: 0,
                        };

                        try {
                            const response = await fetch('http://localhost:3001/api/control', {
                                method: 'POST',
                                headers: {
                                    'Content-Type': 'application/json',
                                },
                                body: JSON.stringify(data),
                            });

                            const result = await response.json();
                            console.log('Cập nhật AUTO thành công:', result);
                        } catch (error) {
                            console.error('Lỗi khi gửi dữ liệu AUTO:', error);
                        }
                    };

                    // Hàm gửi dữ liệu điều khiển đến server
                    const sendControlData = async () => {
                        if (currentMode !== 'manual') return;  // Kiểm tra trạng thái để đảm bảo chỉ gửi dữ liệu trong chế độ MANUAL

                        const fan = document.getElementById('fan').checked ? 1 : 0;
                        const light = document.getElementById('light').checked ? 1 : 0;
                        const roof = document.getElementById('roof').checked ? 1 : 0;

                        const data = {
                            mode: 'manual',
                            fan: fan,
                            light: light,
                            roof: roof,
                        };
                        console.log(data)

                        try {
                            const response = await fetch('http://localhost:3001/api/control', {
                                method: 'POST',
                                headers: {
                                    'Content-Type': 'application/json',
                                },
                                body: JSON.stringify(data),
                            });

                            const result = await response.json();
                            console.log('Cập nhật thành công:', result);
                        } catch (error) {
                            console.error('Lỗi khi gửi dữ liệu:', error);
                        }
                    };

                    // Vô hiệu hoá các nút khi bắt đầu, gửi yêu cầu server, lấy dữ liệu hiển thị
                    toggleControls(true);
                    sendAutoModeData(); 
                    fetchData();
                    setInterval(fetchData, 2000);

                    // Xử lý sự kiện khi chọn chế độ AUTO
                    autoMode.addEventListener('change', function() {
                        if (autoMode.checked) {
                            toggleControls(true); // Vô hiệu hóa các checkbox và nút khi chọn AUTO
                            checkboxes.forEach(checkbox =>checkbox.checked = false);
                            sendAutoModeData(); 
                            console.log("Đã chọn auto");
                        } else {
                            toggleControls(false);  // Cho phép sử dụng các điều khiển khi không chọn AUTO
                            currentMode = 'manual';  // Đặt lại biến trạng thái
                        }
                    });

                    // Xử lý sự kiện khi chọn chế độ MANUAL
                    manualMode.addEventListener('change', function() {
                        if (manualMode.checked) {
                            toggleControls(false); // Kích hoạt các checkbox và nút khi chọn MANUAL
                            checkboxes.forEach(checkbox =>checkbox.checked = false); // Bỏ chọn tất cả checkbox khi chọn MANUAL
                            currentMode = 'manual';
                            console.log("Đã chọn manual");
                        }
                    });

                    // Xử lý sự kiện nút Cập nhật
                    updateButton.addEventListener('click',() => {
                        console.log("Nút Cập nhật được nhấn"); // Kiểm tra xem nút đã được nhấn hay chưa
                        console.log("currentMode là:", currentMode); // Kiểm tra giá trị của currentMode
                        sendControlData(); 
                    });
                    break;
                case 'btn-status':
                    buttons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                    mainContent.innerHTML = `
                        <div id="statisticSection" class = "ThongKe">
                            <div class="statistic-controls" style="display: flex; margin-bottom: 20px;">
                                <!-- Phần 1: Tìm kiếm -->
                                <div style="flex: 1; margin-right: 10px;">
                                    <label for="fromDate">Từ ngày:</label>
                                    <input type="date" id="fromDate" name="fromDate" max="">
                                </div>
                                <div style="flex: 1; margin-right: 10px;">
                                    <label for="toDate">Đến ngày:</label>
                                    <input type="date" id="toDate" name="toDate" max="">
                                </div>
                                <div style="flex: 1; margin-right: 10px;">
                                    <label for="criteria">Tiêu chí thống kê:</label>
                                    <select id="criteria" name="criteria">
                                        <option value="temperature">Nhiệt độ</option>
                                        <option value="humidity">Độ ẩm</option>
                                        <option value="light">Ánh sáng</option>
                                        <option value="solutionTemp">Nhiệt độ dung dịch</option>
                                        <option value="ph">Độ TDS</option>
                                    </select>
                                </div>
                                <div style="flex: 1;">
                                    <button id="searchButton" class="search-button">Tìm kiếm</button>
                                </div>
                            </div>
                            <div id="resultsSection">
                                <!-- Phần 2: Bảng kết quả tìm kiếm -->
                                <table id="resultsTable" border="1" style="width: 100%; border-collapse: collapse; text-align: center;">
                                    <thead>
                                        <tr>
                                            <th>Thời gian</th>
                                            <th id="valueHeader">Giá trị</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    `;

                    const today = new Date().toISOString().split('T')[0]; // Lấy ngày hiện tại
                    document.getElementById('toDate').setAttribute('max', today);
                
                    // Thêm sự kiện cho nút Tìm kiếm
                    document.getElementById('searchButton').addEventListener('click', function() {
                        const fromDate = document.getElementById('fromDate').value;
                        const toDate = document.getElementById('toDate').value;
                        const criteria = document.getElementById('criteria').value;

                        // Kiểm tra các trường có được điền đầy đủ hay không
                        if (!fromDate || !toDate || !criteria) {
                            alert('Vui lòng điền đầy đủ các trường');
                            return;
                        }

                        // Kiểm tra xem từ ngày có lớn hơn đến ngày không
                        if (new Date(fromDate) > new Date(toDate)) {
                            alert('Từ ngày phải nhỏ hơn hoặc bằng đến ngày');
                            return;
                        }

                        // Cập nhật tiêu đề cột Giá trị dựa trên tiêu chí
                        const valueHeader = document.getElementById('valueHeader');
                        switch (criteria) {
                            case 'temperature':
                                valueHeader.textContent = 'Giá trị (°C)';
                                break;
                            case 'humidity':
                                valueHeader.textContent = 'Giá trị (%)';
                                break;
                            case 'ph':
                                valueHeader.textContent = 'Giá trị (ppm)';
                                break;
                            case 'light':
                                valueHeader.textContent = 'Giá trị (lux)';
                                break;
                            case 'solutionTemp':
                                valueHeader.textContent = 'Giá trị (°C)';
                                break;
                            default:
                                valueHeader.textContent = 'Giá trị';
                        }

                        // Gọi API lấy kết quả tìm kiếm
                        fetch(`http://localhost:3001/api/search?fromDate=${fromDate}&toDate=${toDate}&criteria=${criteria}`)
                        .then(response => response.json())
                        .then(results => {
                            const resultsTableBody = document.getElementById('resultsTable').getElementsByTagName('tbody')[0];
                            console.log(resultsTableBody);

                            // Xóa dữ liệu cũ trong bảng
                            resultsTableBody.innerHTML = '';

                            if (results.length === 0) {
                                resultsTableBody.innerHTML = '<tr><td colspan="2">Không có dữ liệu</td></tr>';
                            } else {
                                results.forEach(result => {
                                    //alert('Tìm kiếm thành công');
                                    //const formattedTime = moment(result.thoi_gian).format('HH:mm:ss DD/MM/YYYY');

                                    const date = new Date(result.thoi_gian);

                                    // Định dạng theo 'HH:mm:ss DD/MM/YYYY'
                                    const formattedTime = date.toLocaleString('vi-VN', {
                                        hour: '2-digit',
                                        minute: '2-digit',
                                        second: '2-digit',
                                        day: '2-digit',
                                        month: '2-digit',
                                        year: 'numeric'
                                    });
                                    
                                    const row = resultsTableBody.insertRow();
                                    row.insertCell(0).textContent = formattedTime;
                                    row.insertCell(1).textContent = result.gia_tri;
                                });
                            }
                        })
                        .catch(error => {
                            console.error('Error fetching data:', error);
                        });
                        
                    });
                    break;
                case 'btn-chart':
                    buttons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                    mainContent.innerHTML = `
                        <div class="chart-container">
                            <!-- Phần nhỏ chứa các nút bấm lựa chọn loại dữ liệu -->
                            <div class="chart-controls">
                                <button id="tempButton" class="active">Nhiệt độ (°C)</button>
                                <button id="humidityButton">Độ ẩm (%)</button>
                                <button id="lightButton">Ánh sáng (Lux)</button>
                                <button id="solutionTempButton">Nhiệt độ dung dịch (°C)</button>
                                <button id="phButton">Độ TDS (ppm)</button>
                            </div>
                            <!-- Phần hiển thị biểu đồ -->
                            <div class="chart-area">
                                <canvas id="lineChart"></canvas>
                            </div>
                        </div>
                    `;
                
                    // Khởi tạo biểu đồ
                    const ctx = document.getElementById('lineChart').getContext('2d');
                    const lineChart = new Chart(ctx, {
                        type: 'line',
                        data: {
                            labels: [],
                            datasets: [{
                                label: 'Nhiệt độ (°C)',
                                data: [],
                                borderColor: 'blue',
                                fill: false
                            }]
                        },
                        options: {
                            scales: {
                                x: { 
                                    display: true, title: { display: true, text: 'Thời gian' } },
                                y: { display: true, title: { display: true, text: 'Nhiệt độ (°C)' } }
                            }
                        }
                    });

                    // Hàm lấy dữ liệu từ cơ sở dữ liệu
                    async function fetchDataChart(tableName) {
                        try {
                            const response = await fetch(`http://localhost:3001/api/LastData?table=${tableName}`);
                            if (!response.ok) {
                                throw new Error('Network response was not ok');
                            }
                            const data = await response.json();
                            return data;
                        } catch (error) {
                            console.error('Fetch data failed:', error);
                            return []; // Trả về mảng rỗng nếu có lỗi
                        }
                    }

                    let updateInterval;
                    let previousData = [];

                    /// Hàm cập nhật dữ liệu cho biểu đồ
                    async function updateChartFromDB(tableName, label, borderColor) {
                        async function fetchAndUpdateData() {
                            const data = await fetchDataChart(tableName);
                            // Chỉ lấy điểm cuối cùng từ dữ liệu mới
                            const latestData = data[data.length - 1];
                            const latestValue = latestData.gia_tri;
                            const latestLabel = new Date(latestData.thoi_gian).toLocaleString();

                            // Giữ 9 điểm cũ và thêm điểm mới
                            if (previousData.length === 9) {
                                previousData.shift(); // Xóa điểm đầu tiên khi đã đủ 9 điểm
                            }
                            previousData.push({ label: latestLabel, value: latestValue });

                            // Cập nhật dữ liệu biểu đồ với 10 điểm (9 cũ và 1 mới)
                            lineChart.data.labels = previousData.map(item => item.label);
                            lineChart.data.datasets[0].data = previousData.map(item => item.value);
                            lineChart.data.datasets[0].label = label;
                            lineChart.data.datasets[0].borderColor = borderColor;
                            
                            let yAxisMax = 100; // Giá trị mặc định
                            if (label === 'Ánh sáng (Lux)') yAxisMax = 2000;
                            else if (label === 'Độ ẩm (%)') yAxisMax = 100;
                            else if (label === 'Độ TDS (ppm)') yAxisMax = 1000;

                            lineChart.options.scales.y.title.text = label;
                            lineChart.options.scales.y = {
                                min: 0,       
                                max: yAxisMax 
                            };
                            lineChart.update();

                        }
                    if (updateInterval) clearInterval(updateInterval);
                    // Gọi hàm lấy và cập nhật dữ liệu ngay lập tức
                    await fetchAndUpdateData();
                    updateInterval=setInterval(fetchAndUpdateData, 1000); 
                    }

                    // Thêm sự kiện click cho các nút để cập nhật biểu đồ với loại dữ liệu tương ứng
                    document.getElementById('tempButton').addEventListener('click', () => {
                        updateChartFromDB('nhiet_do', 'Nhiệt độ (°C)', 'blue');
                    });

                    document.getElementById('humidityButton').addEventListener('click', () => {
                        updateChartFromDB('do_am', 'Độ ẩm (%)', 'green');
                    });

                    document.getElementById('lightButton').addEventListener('click', () => {
                        updateChartFromDB('anh_sang', 'Ánh sáng (Lux)', 'yellow');
                    });

                    document.getElementById('solutionTempButton').addEventListener('click', () => {
                        updateChartFromDB('nhiet_do_dung_dich', 'Nhiệt độ dung dịch (°C)', 'red');
                    });

                    document.getElementById('phButton').addEventListener('click', () => {
                        updateChartFromDB('do_tds', 'Độ TDS (ppm)', 'purple');
                    });

                    // Khởi động biểu đồ với dữ liệu nhiệt độ mặc định
                    updateChartFromDB('nhiet_do', 'Nhiệt độ (°C)', 'blue');
                    break;
                case 'btn-logout':
                const confirmLogout = confirm('Bạn có chắc chắn muốn đăng xuất không?');
                if (confirmLogout) {
                    // Chuyển hướng đến trang login.html
                    setTimeout(() => {
                        window.location.href = 'login.html';
                    }, 1000); // Thay đổi thời gian delay nếu cần (1000ms = 1 giây)
                }
                break;
            }
        });
    });
});
$$





