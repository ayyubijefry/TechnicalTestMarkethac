Penjelasan:

- WITH monthly_sales AS (...): Bagian ini adalah Common Table Expression (CTE) yang membuat tabel sementara bernama monthly_sales. CTE ini bertujuan untuk menghitung total kuantitas terjual dan total penjualan per bulan untuk setiap produk.

- FORMAT_TIMESTAMP('%Y-%m', orders.created_at) AS order_month: Fungsi FORMAT_TIMESTAMP mengonversi kolom created_at dari tabel orders menjadi format string YYYY-MM (tahun-bulan).

- products.id AS product_id, products.name AS product_name: Mengambil kolom id dan name dari tabel products untuk mengidentifikasi setiap produk secara unik dan memberikan nama produk pada hasil.

- SUM(orders.num_of_item) AS total_quantity_sold: Menghitung total jumlah barang yang terjual (num_of_item) untuk setiap kombinasi produk dan bulan menggunakan fungsi agregasi SUM.

- SUM(order_items.sale_price * orders.num_of_item) AS total_sales: Menghitung total penjualan untuk setiap produk dalam satu bulan dengan mengalikan harga jual (sale_price) dengan jumlah item (num_of_item), lalu menjumlahkannya.

- GROUP BY order_month, products.id, products.name: GROUP BY mengelompokkan data berdasarkan bulan (order_month), ID produk (products.id), dan nama produk (products.name) untuk memastikan agregasi dilakukan pada level ini.

- top_products AS (...): Bagian ini adalah CTE kedua, yang membuat tabel sementara bernama top_products. Tujuan utamanya adalah untuk menentukan peringkat produk berdasarkan total penjualan dalam setiap bulan.

- RANK() OVER (PARTITION BY order_month ORDER BY total_sales DESC) AS rank: Fungsi analitik RANK() menghitung peringkat produk dalam setiap bulan (PARTITION BY order_month), diurutkan berdasarkan total penjualan dari tertinggi ke terendah (ORDER BY total_sales DESC). Memberikan nomor urut (rank) kepada setiap produk dalam bulan yang sama.

- SELECT order_month, product_id, product_name, total_quantity_sold, total_sales: Query akhir memilih kolom-kolom penting dari tabel top_products

- WHERE rank = 1: Filter ini memilih hanya produk dengan peringkat pertama (rank = 1) berdasarkan total penjualan dalam setiap bulan.

- ORDER BY order_month DESC: Mengurutkan hasil akhir berdasarkan order_month dalam urutan menurun (dari bulan terbaru ke bulan paling lama).
