---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background:
# apply any windi css classes to the current slide
class: 'text-center'
position: 'center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: true
---

# Nhập môn mô hình hóa thống kê
## Nhóm 2
## Chương 11
Thành viên: - Nguyễn Chí Thanh
            - Đoàn Đại Thanh Long
            - Nguyễn Hữu Tuấn Nghĩa
            - Nguyễn Mạnh Linh

<style>
h1 {
  background-color: #2B90B6;
  font-size: 12px;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

# MỤC LỤC

## 11.1 Giới thiệu và tổng quan
## 11.2 Mô hình hóa dữ liệu liên tục dương
## 11.3 Phân phối Gamma
## 11.4 Phân phối Inverse Gaussian
## 11.5 Link function
## 11.6 Ước lượng tham số phân tán
## 11.7 Case Study
## 11.8 Các hàm R liên quan
## 11.9 Tổng kết


<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# 11.1 Giới thiệu và tổng quan

- Mô hình hóa cho dữ liệu liên tục dương. Ví dụ: Các đại lượng vật lý: nhiệt độ (tuyệt đối), áp suất,...
- Hai phân phối phổ biến được chọn để mô hình hóa dữ liệu liên tục dương là phân phối Gamma và phân phối Inverse Gaussian
- 11.2 giới thiệu quá trình mô hình hóa dữ liệu liên tục dương
- 11.3 bàn luận về phân phối Gamma
- 11.4 bàn luận về phân phối Inverse Gaussian
- 11.5 Lựa chọn link function cho các phân phối trên
- 11.6 Ước lượng $\phi$ của các phân phối
- 11.7 trình bày một số Case Study
- 11.8 tổng hợp một số hàm trong R để fit GLM với hai phân phối Gamma và Inverse Gaussian
- 11.9 Tổng kết

---

# 11.2 Mô hình hóa dữ liệu liên tục dương

- Trong nhiều ứng dụng, biến phản hồi liên tục và dương. Các biến này thường lệch phải, ranh giới giới giới hạn 0 là đuôi bên trái của phân phối

- Hệ quả ranh giới giới hạn tại 0:
$\sigma \rightarrow 0$ khi $\mu \rightarrow 0$

- Dữ liệu liên tục dương thường có mối quan hệ trung bình - phương sai tăng. Ví dụ các hàm phương sai:

$V(\mu)=\mu^2$ hoặc $V(\mu)=\mu^3$


- Một tập dữ liệu nằm trong chuỗi các nghiên cứu về sinh khối thực vật ở lục địa Á - Âu là dữ liệu nghiên cứu về lá của cây chanh. Dataset ```lime``` trong package ```GLMsData```

- Tập dữ liệu trên nghiên cứu tính khối lượng của tán lá dựa trên đường kính của tán lá

- Ngoài ra còn thông tin tuổi của cây

---

# 11.2 Mô hình hóa dữ liệu liên tục dương

- Khối lượng của tán lá tỷ lệ với diện tích bề mặt của tán lá

- Đường kính của tán lá có liên quan đến đường kính của tán lá

- Một tán lá có thể xem như dạng gần một quả cầu. $\mu$ tỷ lệ thuận với $4\pi (\dfrac{d}{2})^2=4\pi d^2 \Rightarrow \log y \propto \log \pi + 2 \log d$

- Code:

```python
library(GLMsData); data(lime); str(lime)
```

```python
'data.frame': 385 obs. of 4 variables:
$ Foliage: num 0.1 0.2 0.4 0.6 0.6 0.8 1 1.4 1.7 3.5 ...
$ DBH : num 4 6 8 9.6 11.3 13.7 15.4 17.8 18 22 ...
$ Age : int 38 38 46 44 60 56 72 74 68 79 ...
$ Origin : Factor w/ 3 levels "Coppice","Natural",..: 2 2 2 2 2 2 2
2 2 2 ...
```

---
layout: two-cols
---

# 11.2 Mô hình hóa dữ liệu liên tục dương

<img src="images/lime.png" width="400" height="350">

::right::

- Biến phản hồi (khối lượng tán lá) luôn luôn dương

- Phương sai của biến phản hồi tăng khi trung bình tăng

- Có mối liên hệ giữa khối lượng tán lá và DBH, khối lượng tán lạ và tuổi


- Không có sự khác nhau nhiều về dữ liệu khối lượng tán lá theo nguồn gốc cây

---

# 11.3 Phân phối Gamma

- Phân phối Gamma là một phân phối thuộc họ phân phối xác suất có hai tham số.

- Phân phối Gamma có ba dạng

  - Tham số định dạng $\alpha$ và tham số phạm vi $\beta$:

  $$\mathcal{P}\big(y;\alpha,\beta\big)=\dfrac{y^{\alpha-1}\exp(-y/\beta)}{\Gamma(\alpha)\beta^{\alpha}}$$

  - Tham số định dạng $k=\alpha$ và tham số phạm vi ngược $\theta = \dfrac{1}{\beta}$


  $$\mathcal{P}\big(y;k,\theta\big)=\dfrac{y^{k-1}\theta^{k}\exp(-y\theta)}{\Gamma(k)}$$

---

# 11.3 Phân phối Gamma

- Theo trung bình $\mu$ và tham số phân tán $\phi$

$$\mathrm{E}\lbrack y \rbrack=\alpha\beta$$
$$\mathrm{var}\lbrack y \rbrack=\alpha \beta^2$$

$$\alpha=\frac{1}{\phi}, \beta=\mu\phi$$

$$\mathcal{P}\big(y;\mu,\phi\big)=\Bigg(\frac{y}{\phi \mu} \Bigg)^{1/\phi}\frac{1}{y}\exp\Bigg(-\frac{y}{\phi\mu}\Bigg)\frac{1}{\Gamma(1/\phi)}$$

$$\mathcal{P}\big(y;\mu,\phi\big)=\frac{\phi^{\phi}}{y\Gamma(1/\phi)}\exp\Bigg(-\frac{y}{\phi\mu} + \frac{1}{\phi}\log \frac{y}{\mu}\Bigg)$$

$$\Rightarrow \begin{cases} \theta = \dfrac{1}{\mu} \\ \kappa(\theta) = \log \dfrac{y}{\mu}=\log(y\theta) \end{cases}$$

$$\begin{cases} d(y, \mu) = 2 \lbrace t(y, y) - t(y, \mu) \rbrace = 2  \lbrace -1 + \log 1 + (\dfrac{y}{\mu} - \log \dfrac{y}{\mu}) \rbrace =2\lbrace - \log \dfrac{y}{\mu} + \dfrac{y - \mu}{\mu} \rbrace \\ V(\mu)=\mu^2\end{cases}$$

---

# 11.3 Phân phối Gamma

- Hàm link function tiêu chuẩn

$$\eta = \dfrac{1}{\mu}$$

- Hàm phương sai:

$$V(\mu)=\mu^2$$

- Độ lệch phần dư:

$$D(y, \hat{\mu})=\sum_{i=1}^nw_id(y_, \hat{\mu}_i)$$

$D(y, \hat{\mu}) \sim \chi_{n-p'}^2$ 
nếu điều kiện xấp xỉ hàm yên ngựa được thỏa mãn. Đối với phân phối Gamma điều kiện xấp xỉ hàm yên ngựa được thỏa mãn khi $\phi \leq \dfrac{1}{3}$

- Ít khi biết trước $\phi$



---

# 11.3 Phân phối Gamma

- Mối quan hệ với phân phối Poisson:

- Với các sự kiện xảy ra tuân theo phân phối Poisson, khoảng thời gian giữa hai sự kiện liên tiếp xảy ra tuân theo phân phối Gamma