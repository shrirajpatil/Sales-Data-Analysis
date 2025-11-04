use sales
switched to db sales


1) Calculate the total sales amount for each product category.

db.sales.aggregate([
  { $group: { _id: "$category", totalSales: { $sum: "$amount" } } }
])
{
  _id: 'Electronics',
  totalSales: 3300
}
{
  _id: 'Fashion',
  totalSales: 800
}


2) Determine the month-wise total sales amount.

db.sales.aggregate([
  { $addFields: { month: { $substr: ["$date", 5, 2] } } },
  { $group: { _id: "$month", totalSales: { $sum: "$amount" } } },
  { $sort: { _id: 1 } }
])
{
  _id: '01',
  totalSales: 1000
}
{
  _id: '02',
  totalSales: 1400
}
{
  _id: '03',
  totalSales: 1300
}
{
  _id: '04',
  totalSales: 400
}


3) Identify the highest-selling product (by revenue).

db.sales.aggregate([
  { $group: { _id: "$product", totalSales: { $sum: "$amount" } } },
  { $sort: { totalSales: -1 } },
  { $limit: 1 }
])
{
  _id: 'Laptop',
  totalSales: 1650
}


4) Find the average sale amount across all transactions.

db.sales.aggregate([
  { $group: { _id: null, avgSaleAmount: { $avg: "$amount" } } }
])
{
  _id: null,
  avgSaleAmount: 455.55555555555554
}


5) Count the number of sales made in each month.

db.sales.aggregate([
  { $addFields: { month: { $substr: ["$date", 5, 2] } } },
  { $group: { _id: "$month", totalSalesCount: { $sum: 1 } } },
  { $sort: { _id: 1 } }
])
{
  _id: '01',
  totalSalesCount: 2
}
{
  _id: '02',
  totalSalesCount: 3
}
{
  _id: '03',
  totalSalesCount: 2
}
{
  _id: '04',
  totalSalesCount: 2
}


6) Determine the total sales per region.

db.sales.aggregate([
  { $group: { _id: "$region", totalSales: { $sum: "$amount" } } }
])
{
  _id: 'West',
  totalSales: 1850
}
{
  _id: 'North',
  totalSales: 1300
}
{
  _id: 'South',
  totalSales: 900
}
{
  _id: 'East',
  totalSales: 50
}


7) Identify the top 3 highest revenue-generating products.

db.sales.aggregate([
  { $group: { _id: "$product", totalSales: { $sum: "$amount" } } },
  { $sort: { totalSales: -1 } },
  { $limit: 3 }
])
{
  _id: 'Laptop',
  totalSales: 1650
}
{
  _id: 'TV',
  totalSales: 1000
}
{
  _id: 'Mobile',
  totalSales: 500
}


8) Find the total number of sales transactions per category.

db.sales.aggregate([
  { $group: { _id: "$category", salesCount: { $sum: 1 } } }
])
{
  _id: 'Fashion',
  salesCount: 4
}
{
  _id: 'Electronics',
  salesCount: 5
}


9) Determine the average sales amount for each region.

db.sales.aggregate([
  { $group: { _id: "$region", avgSales: { $avg: "$amount" } } }
])
{
  _id: 'East',
  avgSales: 50
}
{
  _id: 'West',
  avgSales: 925
}
{
  _id: 'North',
  avgSales: 433.3333333333333
}
{
  _id: 'South',
  avgSales: 300
}


10) Find the total sales for Electronics and Fashion categories separately.

db.sales.aggregate([
  { $match: { category: { $in: ["Electronics", "Fashion"] } } },
  { $group: { _id: "$category", totalSales: { $sum: "$amount" } } }
])
{
  _id: 'Electronics',
  totalSales: 3300
}
{
  _id: 'Fashion',
  totalSales: 800
}
