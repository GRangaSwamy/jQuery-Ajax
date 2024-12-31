<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ajax Example API</title>
    <link rel="stylesheet" href="../node_modules/bootstrap/dist/css/bootstrap.css">
    <link rel="stylesheet" href="../node_modules/bootstrap-icons/font/bootstrap-icons.css">
    <script src="../node_modules/jquery/dist/jquery.js"></script>
    <style>
        #main {
            display: flex;
            flex-wrap: wrap;
        }
    </style>
    <script>
        $(function () {
            let products = [];
            let categories = new Set();

            $.ajax({
                method: "GET",
                url: "https://fakestoreapi.com/products",
            })
            .then(function (data) {
                products = data;
                
                $.each(data, function (key, value) {
                    if (!categories.has(value.category)) {
                        categories.add(value.category);
                        $("#lstcat").append(
                            $("<option>", {
                                value: value.category,
                                text: value.category.toUpperCase(),
                            })
                        );
                    }
                });

                displayProducts(products);
            });

            $("#lstcat").change(function () {
                const selectedCategory = $(this).val();
                let filteredProducts;

                if (selectedCategory && selectedCategory !== "Select") {
                    filteredProducts = products.filter(function (product) {
                        return product.category === selectedCategory;
                    });
                } else {
                    filteredProducts = products;
                }   
                 displayProducts(filteredProducts);
            });

            function displayProducts(products) {
                $("#main").empty();
                $.each(products, function (key, value) {
                    let card = `
                        <div class="card m-2 shadow-sm" style="width: 18rem;">
                            <div class="card-header">${value.title}</div>
                            <div class="card-body">
                                <img src="${value.image}" class="card-img-top" alt="${value.title} style = "height:100px;">
                            </div>
                            <div class="card-footer"><button class = "btn btn-outline-primary">Buy now</button>  &emsp;Price : Rs ${value.price}/-</div>
                        </div>
                    `;
                    $("#main").append(card);
                });
            }
        });
    </script>
</head>
<body>
    <h2 class="bg-primary p-3 text-center fs-1"><span class="bi bi-cart4"></span> ShopEasy</h2>
    <div class="row">
        <div class="col-3">
            <select name="select" id="lstcat" class="form-select">
                <option>ALL</option>
            </select>
        </div>
        <div class="col-9" id="main"></div>
    </div>
    <script src="../node_modules/bootstrap/dist/js/bootstrap.bundle.js"></script>
</body>
</html>
