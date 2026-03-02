# 🏠 使用线性回归预测房价  
# 🏠 House Price Prediction using Linear Regression  

[![Python](https://img.shields.io/badge/Python-3.13-blue.svg)](https://www.python.org/)  
[![Pandas](https://img.shields.io/badge/Pandas-2.0+-green.svg)](https://pandas.pydata.org/)  
[![Statsmodels](https://img.shields.io/badge/Statsmodels-0.14+-orange.svg)](https://www.statsmodels.org/)  
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)  

---

## 📌 项目概述  
## 📌 Project Overview  

本项目基于线性回归模型，利用房屋的各种属性（如面积、卧室数、卫生间数、位置、装修状态等）来预测房价。数据集中包含超过500条房屋交易记录，目标是构建一个能够根据房屋特征估算其价格的数学模型。  

This project aims to predict house prices using **Linear Regression** based on various housing attributes. The dataset contains transaction records for over 500 houses, including features such as area, number of bedrooms, bathrooms, location, amenities, and furnishing status. The goal is to build a predictive model that can estimate the price of a house given its characteristics.  

---

## 🎯 分析目标  
## 🎯 Analysis Objective  

本项目的核心目标是建立一个线性回归模型，能够根据房屋属性预测价格。利用训练好的模型，我们可以估算具有以下特征的新房价格：  

The primary objective is to develop a linear regression model that can predict house prices based on property features. Using the trained model, we can estimate the price of a new house with the following attributes:  

| 特征 Feature | 值 Value |
|--------------|----------|
| 面积 Area | 6,500 平方英尺 sq ft |
| 卧室数 Bedrooms | 4 |
| 卫生间数 Bathrooms | 2 |
| 楼层数 Floors | 2 |
| 位于主路 Main Road | 否 No |
| 有客房 Guest Room | 否 No |
| 有地下室 Basement | 是 Yes |
| 有热水器 Hot Water Heater | 是 Yes |
| 有空调 Air Conditioning | 否 No |
| 车位 Parking Spaces | 2 |
| 位于首选社区 Preferred Neighborhood | 是 Yes |
| 装修状态 Furnishing Status | 简装 Semi-furnished |

---

## 📊 数据集描述  
## 📊 Dataset Description  

数据集 `house_price.csv` 包含 **545 条观测值**，特征如下：  

The dataset `house_price.csv` contains **545 observations** with the following features:  

| 列名 Column | 描述 Description | 值 Values |
|-------------|------------------|-----------|
| `price` | 房屋售价 Sale price | 连续值 Continuous |
| `area` | 面积（平方英尺） Area in square feet | 连续值 Continuous |
| `bedrooms` | 卧室数 Number of bedrooms | 1–6 |
| `bathrooms` | 卫生间数 Number of bathrooms | 1–4 |
| `stories` | 楼层数 Number of floors | 1–4 |
| `mainroad` | 是否位于主路 Located on main road? | `yes` / `no` |
| `guestroom` | 是否有客房 Has guest room? | `yes` / `no` |
| `basement` | 是否有地下室 Has basement? | `yes` / `no` |
| `hotwaterheating` | 是否有热水器 Has hot water heater? | `yes` / `no` |
| `airconditioning` | 是否有空调 Has AC? | `yes` / `no` |
| `parking` | 车位数 Number of parking spaces | 0–3 |
| `prefarea` | 是否位于首选社区 Located in preferred neighborhood? | `yes` / `no` |
| `furnishingstatus` | 装修状态 Furnishing status | `furnished`（精装）, `semi-furnished`（简装）, `unfurnished`（毛坯） |

---

## 🧹 数据清洗与准备  
## 🧹 Data Cleaning & Preparation  

- ✅ 创建原始数据的副本，以保留原始数据 Created a copy of the original data to preserve raw data.  
- ✅ 检查缺失值 — **无缺失值** Checked for missing values — **none found**.  
- ✅ 使用 `.describe()` 验证数据无异常值 Verified no unrealistic values using `.describe()`.  
- ✅ 将分类变量转换为 `category` 类型 Converted categorical variables to `category` type.  

---

## 📈 探索性数据分析  
## 📈 Exploratory Data Analysis  

### 关键变量分布  
### Distribution of Key Variables  

- **房价与面积**：均呈**右偏分布**，说明大多数房屋价格/面积适中，但存在少量高价/大面积 outliers。  
  **Price & Area**: Both show **right-skewed distributions**, indicating most houses are moderately priced/sized with a few high-end outliers.  
- **相关性**：散点图显示房价与面积、卫生间数、楼层数、车位数以及各种配套设施之间存在**正相关关系**。  
  **Correlation**: Scatter plots show a **positive correlation** between price and area, bathrooms, stories, parking, and various amenities.  

### 分类变量与房价  
### Categorical Variables & Price  

- 拥有**更多卫生间**、**更多楼层**、**更多车位**以及**位于首选社区**的房屋，其平均价格更高。  
  Houses with **more bathrooms**, **more floors**, **more parking**, and **preferred location** tend to have higher average prices.  
- **精装房**价格高于简装房，简装房价格高于毛坯房。  
  **Furnished houses** command higher prices than semi-furnished, which are higher than unfurnished.  
- 配套设施如**客房**、**地下室**、**热水器**和**空调**也与更高的房价相关。  
  Amenities like **guest room**, **basement**, **hot water heater**, and **AC** also correlate with higher prices.  

---

## 📐 线性回归建模  
## 📐 Linear Regression Modeling  

### 1. 建模数据准备  
### 1. Data Preparation for Modeling  

- 使用 `pd.get_dummies()` 对分类特征创建虚拟变量，设置 `drop_first=True`。  
  Created dummy variables for categorical features using `pd.get_dummies()` with `drop_first=True`.  
- 分离**因变量**（`price`）和**自变量**（所有其他特征）。  
  Separated **dependent variable** (`price`) and **independent variables** (all others).  
- 检查**多重共线性** — 所有相关系数绝对值均 ≤ 0.8，无严重共线性问题。  
  Checked for **multicollinearity** — no correlation coefficient > 0.8, so no severe collinearity.  

### 2. 模型构建  
### 2. Model Building  

- 使用 `sm.add_constant()` 添加**截距项**。  
  Added an **intercept** using `sm.add_constant()`.  
- 使用 `statsmodels` 的 **OLS（普通最小二乘法）** 进行回归。  
  Used **OLS (Ordinary Least Squares)** from `statsmodels`.  

```python
model = sm.OLS(y, X).fit()
```

### 3. 模型结果  
### 3. Model Results  

- **R-squared**: ~0.68 — 模型解释了房价约 **68%** 的方差。  
  **R-squared**: ~0.68 — the model explains about **68%** of the variance in house prices.  
- **显著变量（p < 0.05）**：  
  **Significant predictors (p < 0.05)**:  
  - `area`（面积）  
  - `bathrooms`（卫生间数）  
  - `stories`（楼层数）  
  - `parking`（车位数）  
  - `mainroad_yes`（位于主路）  
  - `guestroom_yes`（有客房）  
  - `basement_yes`（有地下室）  
  - `hotwaterheating_yes`（有热水器）  
  - `airconditioning_yes`（有空调）  
  - `prefarea_yes`（位于首选社区）  

- **不显著变量**：`bedrooms`（卧室数）和 `furnishingstatus_semi-furnished`（简装）在 p < 0.05 水平上不显著，已在优化模型中移除。  
  **Non-significant variables**: `bedrooms` and `furnishingstatus_semi-furnished` were **not statistically significant** at p < 0.05 and were removed in the refined model.  

---

## 🧠 结果解读  
## 🧠 Interpretation  

最终线性回归模型表明，以下因素会**显著增加**房价：  

The final linear regression model suggests that the following factors **significantly increase** house prices:  

- ✅ 更大的面积 Larger area  
- ✅ 更多的卫生间 More bathrooms  
- ✅ 更多的楼层 More floors  
- ✅ 更多的车位 More parking spaces  
- ✅ 位于主路 Located on a main road  
- ✅ 有客房 Has a guest room  
- ✅ 有地下室 Has a basement  
- ✅ 有热水器 Has hot water heating  
- ✅ 有空调 Has air conditioning  
- ✅ 位于首选社区 Located in a preferred neighborhood  

以下因素会**显著降低**房价：  

The factor that **significantly decreases** prices:  

- ❌ 毛坯房 Being unfurnished (`furnishingstatus_unfurnished`)  

---

## 🔮 预测示例  
## 🔮 Prediction Example  

使用优化后的模型，对待预测房屋的价格进行估算：  

Using the refined model, we estimate the price for the target house:  

```python
# 待预测房屋信息
# Target house information
price_to_predict = pd.DataFrame({
    'area': [5600], 'bedrooms': [4], 'bathrooms': [2], 
    'stories': [2], 'mainroad': ['no'], 'guestroom': ['no'],
    'basement': ['yes'], 'hotwaterheating': ['yes'],
    'airconditioning': ['no'], 'parking': 2, 'prefarea': ['yes'],
    'furnishingstatus': ['semi-furnished']
})

# 预测结果
# Prediction result
predicted_value = model.predict(price_to_predict)
```

**预测价格 Predicted Price: 7,071,927**

---

## 🛠️ 依赖库  
## 🛠️ Dependencies  

- Python 3.13  
- pandas  
- numpy  
- matplotlib  
- seaborn  
- statsmodels  

---

## 📄 许可证  
## 📄 License  

本项目采用 MIT 许可证。  
This project is licensed under the MIT License.  

---

## 👩‍💻 作者  
## 👩‍💻 Author  

[connie-debug](https://github.com/connie-debug)
