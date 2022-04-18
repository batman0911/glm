---
marp: true
---

# 11.5 Link Function

---

`Link function` dạng `log` là một trong những `link functions` phổ biến cho mô hình GLMs với phân phối `Gamma` và phân phối `inverse Gaussian` để đảm bảo  $\mu > 0$ và cho mục đích diễn giải


Trong `R`, đối với phân phối `Gamma` và phân phối `inverse Gaussian`, các `link function` được sử dụng là `log`, `identity` và `inverse` (mặc định cho phân phối `Gamma`). Hàm `link = "1/mu^2"` cũng được sử dụng cho phân phối `inverse Gaussian`.

---


### Ví dụ 1
Data small-leaved lime trong dataset `lime`

```{r}
library(GLMsData); data(lime); str(lime)
```

```
'data.frame':	385 obs. of  4 variables:
 $ Foliage: num  0.1 0.2 0.4 0.6 0.6 0.8 1 1.4 1.7 3.5 ...
 $ DBH    : num  4 6 8 9.6 11.3 13.7 15.4 17.8 18 22 ...
 $ Age    : int  38 38 46 44 60 56 72 74 68 79 ...
 $ Origin : Factor w/ 3 levels "Coppice","Natural",..: 2 2 2 2 2 2 2 2 2 2 ...
```
Trong đó

`Foliage` là khối lượng tán lá tính bằng `kg`

`DBH` là đường kính cây tính bằng `cm`

`Age` là tuổi cây tính bằng năm

`Origin` là nguồn gốc của cây: `Coppice`, `Natural` và `Planted`

---
### Ví dụ 1

##### Sử dụng phân phối `Gamma` với các `link functions` khác nhau.

Với `link function` dạng `log`

```r
> lime.log <- glm(Foliage ~ log(DBH), family=Gamma(link="log"),data=lime)
> lime.log$coefficients
```

```r
(Intercept)    log(DBH) 
  -4.707996    1.842207
```
---
### Ví dụ 1
<img src="Rplot05.png">

---
### Ví dụ 1
Với `inverse link function`
```r
> lime.inv <- update(lime.log, family=Gamma(link="inverse"))
```

`R` đưa ra cảnh báo

```
Warning in log(ifelse(y == 0, 1, y/mu)) : NaNs produced
```

`R` không thể tìm được các điểm bắt đầu phù hợp với mô hình, chúng ta có thể cung cấp cho hàm `glm()` các điểm bắt đầu sử dụng `mustart` (cho thang đo của data) hoặc `etastart` cho thang dự đoán tuyến tính

```r
> lime.inv <- update(lime.log, family=Gamma(link="inverse"), mustart=fitted(lime.log))
```
`R` tiếp tục báo lỗi như trên vậy nên chúng ta không xem xét mô hình này nữa. Hoàn toàn tương tự với `link function` `identity`

