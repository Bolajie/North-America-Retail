# North America Retail
 
 ##  Project Overview 
 North America Retail is a major retail company operating in multiple locations, offering a wide range of products to different customer groups. They focus on excellent customer service and a smooth shopping experience. As a data analyst, your job is to analyse their sales data to uncover key insights on profitability, business performance, products, and customer behaviour. You'll work with a dataset containing details on products, customers, sales, profits, and returns. Your findings will help identify areas for improvement and suggest strategies to boost efficiency and profitability.

 ## Problem Statement 

Efficient delivery and profitability are critical factors in optimizing business performance and customer satisfaction. 
This analysis aims to evaluate delivery efficiency and profitability across different product subcategories and customer segments:  
1. Determining the **average delivery time** for different product subcategories and customer segments.  
2. Identifying the **fastest and slowest delivered products** to assess supply chain efficiency.  
3. Analyzing profitability by identifying the **most profitable product subcategory and segment**.  
4. Recognizing the **top 5 customers contributing the most profit** to improve customer relationship management.  
5. Calculating the **total number of products by subcategory** to assess inventory distribution.  

---
 ## Data Source:
**Retail Sales  records was used in this analysis. Check it out** [here](Retail-Supply-Chain-Sales-Analysis.csv)
---
 ## METHODOLOGY
### Heres are my analysis [Sales Analysis](SALES.sql)

### **1. Data Collection**
- The dataset was sourced from `[CAPSTONE PROJECT].[dbo].[Sales Analysis]`, which contains sales-related records.  
- Ensured data consistency by checking for missing values, duplicates, and inconsistencies in delivery dates and product categories.  

### **2. Data Modeling**
- The dataset was structured into a **star schema** by separating it into **dimension tables** (`DimOrders`, `DimCustomer`, `DimLocation`, `DimProduct`) and a **fact table** (`SalesFact`).  
- The dimension tables store descriptive attributes, while the fact table contains transactional data.  
- This approach optimizes queries for better performance and analytical efficiency.  
![model](<Screenshot 2025-03-09 182312.png>)
### **3. Data Cleaning & Preparation**
- **Date Formatting:** Ensured date columns were in the appropriate format to calculate delivery days.  
- **Duplicate Handling:**  
  - Used `ROW_NUMBER()` with `PARTITION BY` to identify and remove duplicate records in each dimension table and the fact table.  
- **Surrogate Key Creation:**  
  - Identified duplicate `Product_ID`s in `DimProduct`, which led to the creation of a **surrogate key** (`PRODUCT_KEY`) in both `DimProduct` and `SalesFact` to ensure proper relationships.  
- **Rounding Measures:**  
  - Used `ROUND()` to ensure numerical fields (`Sales`, `Discount`, `Profit`) were rounded to two decimal places for consistency.  

### **4. Data Integration**
- Used `UPDATE` statements to map foreign keys, such as `Product_Key` in `SalesFact` referencing `DimProduct`.  
- Ensured that order dates in `SalesFact` were updated to match those in `DimOrders`.  

### **5. Sales & Profitability Analysis**
- **Delivery Time Analysis:**  
  - Used `DATEDIFF(DAY, Order_Date, Ship_Date)` to analyze average delivery time across different subcategories and customer segments.  
- **Profitability Insights:**  
  - Identified the **most and least profitable products** and **subcategories** by summing the `Profit` field.  
  - Compared the total loss incurred by unprofitable products.  
- **Revenue & Discount Impact:**  
  - Evaluated the impact of discounts on profitability using `AVG(Discount)`.  
  - Analyzed total revenue, quantity sold, and profitability per subcategory.  
- **Customer Segmentation Analysis:**  
  - Grouped customers by segment (`Consumer`, `Corporate`, `Home Office`) and measured their contribution to total profit.  

### **6. Business Insights Derived**
- **Most Profitable Subcategory:** Chairs generated the highest total profit.  
- **Most Unprofitable Subcategory:** Tables incurred the highest losses.  
- **Best-Selling vs. Most Profitable:**  
  - Furnishings had the highest sales quantity but lower profitability.  
  - Tables had high sales revenue but significant losses, indicating pricing or cost inefficiencies.  
- **Top Customers by Profitability:** Identified the highest revenue-generating customers.  
---
## **Key Insights**

### **1. Delivery Time Analysis**
- **Longest Delivery Time by Category:**  
  - **Tables** have the highest average delivery time at **36 days**.  
  - **Furnishings** follow closely at **34 days**, while **Chairs** and **Bookcases** both average **32 days**.  
- **Delivery Time by Customer Segment:**  
  - **Home Office** customers experience the shortest delivery time (**31 days**).  
  - **Consumer** customers receive their orders in **34 days**, while **Corporate** customers take the longest at **35 days**.  
- **Longest Delivery Time by Product:**  
  - **Bevis Conference Table** has the highest delivery time at **167 days**.  
  - **Bush Mission Conference Table** and **Global Enterprise Tilt Chair** both average **123 days**.  
  - **Bush Cubix Collection Bookcase (Fully Assembled)** takes **103 days**, and **Career Cubicle Clock** follows closely at **102 days**.  

## **2. Profitability & Sales Performance**
### **2.1 Profitability by Subcategory**
- **Top Profitable Subcategories:**  
  - **Chairs** generate the most profit at **$26,670.67**.  
  - **Furnishings** follow at **$12,936.90**.  
- **Least Profitable Subcategories:**  
  - **Bookcases** incurred **$3,550.04** in losses.  
  - **Tables** suffered the highest losses at **$12,761.31**.  

### **2.2 Profitability by Customer Segment**
- **Corporate customers** bring the highest profit at **$10,649.44**.  
- **Consumer customers** contribute **$7,411.15**.  
- **Home Office customers** generate the least profit at **$5,235.52**.  

### **2.3 Most Profitable Customers**
- **Top five customers by profit contribution:**  
  1. **Laura Armstrong** - $1,146.49  
  2. **Quincy Jones** - $1,013.13  
  3. **Joe Elijah** - $968.08  
  4. **Bill Donatelli** - $805.98  
  5. **Anne McFarland** - $799.45  

### **2.4 Sales Volume vs. Profitability**
- **Best-Selling Category:** **Furnishings** had **3,535 units sold**, but profitability remains lower than Chairs.  
- **Highest Sales Revenue:** **Chairs** generated **$327,986.03**, with **2,344 units sold**, making it the second-highest selling category.  
- **Tables Have the Most Losses:**  
  - Despite **$122,943.59** in sales, the **low sales quantity** and **high average discount** led to negative profit.  
- **Bookcases Performance:**  
  - Earned **$103,713.63** with **840 units sold**, ranking third in sales.  
- **Furnishings Performance:**  
  - Despite being the best-selling category, it generated the lowest revenue at **$91,089.46**, indicating a high sales volume but low revenue per unit.  

### **2.5 Losses by Subcategory**
- **Tables** had the highest loss of **$14,592**.  
- **Bookcases** had losses amounting to **$7,020.03**, which is nearly half of its total profit.  
- **Chairs** also incurred losses of **$5,418**, while **Furnishings** had the lowest loss at **$3,037**, making it a low-risk, low-reward subcategory.  

## **3. Issues in the Table Subcategory**
- **All table products are unprofitable**, meaning their total cost exceeds revenue.  
- **High sales volume does not guarantee profitability**, as even the best-selling tables still incur losses.  
- **Pricing and cost management require optimization**, as excessive discounts alone do not fully explain the losses.  
 
---

## **2. Conclusion**
The analysis highlights that while some product categories drive high sales, their profitability is negatively affected by discounts and operational inefficiencies. Additionally, certain customer segments contribute more to profits than others, suggesting opportunities for targeted marketing. The data also indicates a need to optimize delivery times, particularly for specific product categories.

---

## **3. Recommendations**
1. **Reevaluate Pricing & Discounts for Unprofitable Products**  
   - Adjust pricing strategies for high-loss products like **Tables** to improve profitability.  
   - Implement a data-driven discounting approach to prevent revenue losses.  

2. **Optimize Inventory & Delivery**  
   - Identify the causes of long delivery times and streamline the supply chain.  
   - Improve inventory forecasting to meet demand efficiently.  

3. **Target High-Value Customers & Locations**  
   - Increase marketing efforts toward **Corporate customers**, as they generate the most profit.  
   - Focus sales strategies on high-performing locations to maximize returns.  

4. **Improve Discount Strategies**  
   - Limit excessive discounts on products with already low profit margins.  
   - Introduce loyalty programs or bundled offers instead of deep discounts.  

By implementing these strategies, the business can enhance overall profitability while ensuring customer satisfaction and operational efficiency. 🚀


