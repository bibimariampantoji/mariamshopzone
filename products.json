
// ===============================
// SHOPZONE JAVASCRIPT
// ===============================

// Select HTML elements

const productContainer = document.getElementById("productContainer");
const searchInput = document.getElementById("searchInput");
const searchBtn = document.getElementById("searchBtn");

const cartButton = document.getElementById("cartButton");
const cartSidebar = document.getElementById("cartSidebar");
const cartOverlay = document.getElementById("cartOverlay");
const closeCart = document.getElementById("closeCart");

const cartItems = document.getElementById("cartItems");
const cartCount = document.getElementById("cartCount");
const cartTotal = document.getElementById("cartTotal");

const checkoutBtn = document.getElementById("checkoutBtn");

// Store products and cart

let products = [];

let cart = JSON.parse(localStorage.getItem("shopzoneCart")) || [];

let selectedCategory = "All";


// ===============================
// FETCH PRODUCTS FROM JSON
// ===============================

async function loadProducts() {

    try {

        const response = await fetch("products.json");

        if (!response.ok) {
            throw new Error("Unable to load products");
        }

        products = await response.json();

        displayProducts(products);

        updateCart();

    } catch (error) {

        console.error("Error:", error);

        productContainer.innerHTML = `
            <p class="no-products">
                Unable to load products.
                Please run the project using a local server.
            </p>
        `;

    }

}


// ===============================
// DISPLAY PRODUCTS
// ===============================

function displayProducts(productList) {

    productContainer.innerHTML = "";

    if (productList.length === 0) {

        productContainer.innerHTML = `
            <p class="no-products">
                No products found!
            </p>
        `;

        return;

    }

    productList.forEach(product => {

        const card = document.createElement("div");

        card.className = "product-card";

        card.innerHTML = `

            <div class="product-image">

                <img
                    src="${product.image}"
                    alt="${product.name}"
                    loading="lazy"
                >

            </div>

            <div class="product-info">

                <span class="product-category">
                    ${product.category}
                </span>

                <h3>${product.name}</h3>

                <div class="rating">

                    ${"★".repeat(Math.floor(product.rating))}
                    ${product.rating % 1 >= 0.5 ? "½" : ""}
                    <span>${product.rating}/5</span>

                </div>

                <div class="product-bottom">

                    <span class="product-price">
                        ₹${product.price.toLocaleString("en-IN")}
                    </span>

                    <button
                        class="add-cart"
                        data-id="${product.id}"
                    >
                        <i class="fa-solid fa-cart-plus"></i>
                        Add to Cart
                    </button>

                </div>

            </div>

        `;

        productContainer.appendChild(card);

    });

}


// ===============================
// ADD TO CART
// ===============================

productContainer.addEventListener("click", function(event) {

    const button = event.target.closest(".add-cart");

    if (!button) return;

    const productId = Number(button.dataset.id);

    const existingProduct = cart.find(
        item => item.id === productId
    );

    if (existingProduct) {

        existingProduct.quantity++;

    } else {

        const product = products.find(
            item => item.id === productId
        );

        if (product) {

            cart.push({
                ...product,
                quantity: 1
            });

        }

    }

    updateCart();

    // Button feedback

    button.innerHTML = '<i class="fa-solid fa-check"></i> Added';

    setTimeout(() => {

        button.innerHTML = `
            <i class="fa-solid fa-cart-plus"></i>
            Add to Cart
        `;

    }, 1000);

});


// ===============================
// UPDATE CART
// ===============================

function updateCart() {

    cartItems.innerHTML = "";

    let total = 0;
    let totalItems = 0;

    if (cart.length === 0) {

        cartItems.innerHTML = `
            <p class="empty-cart">
                Your cart is empty!
            </p>
        `;

    }

    cart.forEach(item => {

        total += item.price * item.quantity;

        totalItems += item.quantity;

        const cartItem = document.createElement("div");

        cartItem.className = "cart-item";

        cartItem.innerHTML = `

            <img
                src="${item.image}"
                alt="${item.name}"
            >

            <div class="cart-item-info">

                <h4>${item.name}</h4>

                <p>
                    ₹${(item.price * item.quantity).toLocaleString("en-IN")}
                </p>

                <div class="quantity-controls">

                    <button
                        class="decrease"
                        data-id="${item.id}"
                        aria-label="Decrease quantity"
                    >−</button>

                    <span>${item.quantity}</span>

                    <button
                        class="increase"
                        data-id="${item.id}"
                        aria-label="Increase quantity"
                    >+</button>

                </div>

            </div>

            <button
                class="remove-item"
                data-id="${item.id}"
                aria-label="Remove product"
            >
                <i class="fa-solid fa-trash"></i>
            </button>

        `;

        cartItems.appendChild(cartItem);

    });

    // Update total and count

    cartCount.textContent = totalItems;

    cartTotal.textContent =
        "₹" + total.toLocaleString("en-IN");

    // Save cart to LocalStorage

    localStorage.setItem(
        "shopzoneCart",
        JSON.stringify(cart)
    );

}


// ===============================
// CART QUANTITY AND REMOVE
// ===============================

cartItems.addEventListener("click", function(event) {

    const button = event.target.closest("button");

    if (!button) return;

    const productId = Number(button.dataset.id);

    const product = cart.find(
        item => item.id === productId
    );

    if (!product) return;

    if (button.classList.contains("increase")) {

        product.quantity++;

    }

    else if (button.classList.contains("decrease")) {

        product.quantity--;

        if (product.quantity <= 0) {

            cart = cart.filter(
                item => item.id !== productId
            );

        }

    }

    else if (button.classList.contains("remove-item")) {

        cart = cart.filter(
            item => item.id !== productId
        );

    }

    updateCart();

});


// ===============================
// OPEN CART
// ===============================

function openCart() {

    cartSidebar.classList.add("active");

    cartOverlay.classList.add("active");

}


// ===============================
// CLOSE CART
// ===============================

function hideCart() {

    cartSidebar.classList.remove("active");

    cartOverlay.classList.remove("active");

}

cartButton.addEventListener("click", openCart);

closeCart.addEventListener("click", hideCart);

cartOverlay.addEventListener("click", hideCart);


// Close cart using Escape key

document.addEventListener("keydown", function(event) {

    if (event.key === "Escape") {

        hideCart();

    }

});


// ===============================
// SEARCH PRODUCTS
// ===============================

function searchProducts() {

    const searchText = searchInput.value
        .toLowerCase()
        .trim();

    const filteredProducts = products.filter(product => {

        const matchesSearch =
            product.name.toLowerCase().includes(searchText) ||
            product.category.toLowerCase().includes(searchText);

        const matchesCategory =
            selectedCategory === "All" ||
            product.category === selectedCategory;

        return matchesSearch && matchesCategory;

    });

    displayProducts(filteredProducts);

}

searchBtn.addEventListener("click", searchProducts);

searchInput.addEventListener("input", searchProducts);


// ===============================
// CATEGORY FILTER
// ===============================

const categoryButtons = document.querySelectorAll(".filter-btn");

categoryButtons.forEach(button => {

    button.addEventListener("click", function() {

        categoryButtons.forEach(btn => {
            btn.classList.remove("active");
        });

        this.classList.add("active");

        selectedCategory = this.dataset.category;

        searchProducts();

    });

});


// ===============================
// CHECKOUT BUTTON
// ===============================

checkoutBtn.addEventListener("click", function() {

    if (cart.length === 0) {

        alert("Your cart is empty!");

        return;

    }

    alert(
        "Thank you for shopping with ShopZone!\n\n" +
        "Your order total is ₹" +
        cart.reduce(
            (sum, item) => sum + item.price * item.quantity,
            0
        ).toLocaleString("en-IN") +
        "\n\nThis is a demo checkout. No payment has been processed."
    );

});


// ===============================
// NEWSLETTER
// ===============================

const newsletterForm = document.getElementById("newsletterForm");

newsletterForm.addEventListener("submit", function(event) {

    event.preventDefault();

    alert("Thank you for subscribing to ShopZone!");

    newsletterForm.reset();

});


// ===============================
// START WEBSITE
// ===============================

loadProducts();
