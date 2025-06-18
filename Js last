const products = [
    {
    id: 1,
        name: "Classic T-Shirt",
        images: [
            "https://placehold.co/400x300/F0F8FF/333?text=T-Shirt+Front",
            "https://placehold.co/400x300/ADD8E6/333?text=T-Shirt+Back",
        ],
        price: 19.99,
        variants: [
            { id: "s", name: "Small", stock: 10 },
            { id: "m", name: "Medium", stock: 0 },
            { id: "l", name: "Large", stock: 5 },
        ],
    },
    {
        id: 2,
        name: "Ergonomic Office Chair",
        images: [
            "https://placehold.co/400x300/E6E6FA/333?text=Chair+View+1",
            "https://placehold.co/400x300/B0C4DE/333?text=Chair+View+2",
        ],
        price: 149.00,
        variants: [
            { id: "black", name: "Black", stock: 3 },
            { id: "white", name: "White", stock: 0 },
        ],
    },
    {
        id: 3,
        name: "Wireless Headphones",
        images: [
            "https://placehold.co/400x300/FFF0F5/333?text=Headphones+Left",
            "https://placehold.co/400x300/FFDAB9/333?text=Headphones+Right",
        ],
        price: 79.50,
        variants: [
            { id: "std", name: "Standard", stock: 15 },
        ],
    },
    {
        id: 4,
        name: "Smartwatch",
        images: [
            "https://placehold.co/400x300/DDA0DD/333?text=Smartwatch+Screen",
            "https://placehold.co/400x300/BA55D3/333?text=Smartwatch+Side",
        ],
        price: 199.99,
        variants: [
            { id: "40mm", name: "40mm", stock: 8 },
            { id: "44mm", name: "44mm", stock: 2 },
        ],
    },
    {
        id: 5,
        name: "Yoga Mat",
        images: [
            "https://placehold.co/400x300/E0BBE4/333?text=Yoga+Mat+Front",
            "https://placehold.co/400x300/957DAD/333?text=Yoga+Mat+Rolled",
        ],
        price: 29.95,
        variants: [
            { id: "pink", name: "Pink", stock: 7 },
            { id: "blue", name: "Blue", stock: 0 },
        ],
    },
    {
        id: 6,
        name: "Coffee Maker",
        images: [
            "https://placehold.co/400x300/C7CEEA/333?text=Coffee+Maker+1",
            "https://placehold.co/400x300/B8F2E6/333?text=Coffee+Maker+2",
        ],
        price: 89.00,
        variants: [
            { id: "standard", name: "Standard", stock: 12 },
        ],
    },
];

let cart = [];
let carouselTrack;
let currentIndex = 0;
let visibleCards = 1;
let cardWidth = 0;

function updateAddToCartButton(cardElement, product) {
  const selectedVariantId = cardElement.dataset.selectedVariantId;
  const selectedVariant = product.variants.find(v => v.id === selectedVariantId);
  const isOutOfStock = selectedVariant ? selectedVariant.stock === 0 : true;

  const btn = cardElement.querySelector(".add-to-cart-button");
  btn.disabled = isOutOfStock;
  btn.textContent = isOutOfStock ? "Out of Stock" : "Add to Cart";
  btn.classList.toggle("out-of-stock", isOutOfStock);
  btn.classList.toggle("available", !isOutOfStock);
}

function createProductCard(product) {
  const card = document.createElement("div");
  card.className = "product-card carousel-card-item";
  card.dataset.selectedVariantId = product.variants[0]?.id;

  const img = document.createElement("img");
  img.src = product.images[0];
  img.className = "product-card-image";
  img.alt = product.name;
  card.appendChild(img);

  const details = document.createElement("div");
  details.className = "product-details";
  card.appendChild(details);

  details.innerHTML = `
    <h3 class="product-name">${product.name}</h3>
    <p class="product-price">$${product.price.toFixed(2)}</p>
  `;

  const variantDiv = document.createElement("div");
  variantDiv.className = "variant-options";
  details.appendChild(variantDiv);

  if (product.variants.length > 1) {
    const select = document.createElement("select");
    select.className = "variant-select";
    product.variants.forEach(variant => {
      const option = document.createElement("option");
      option.value = variant.id;
      option.textContent = `${variant.name} (${variant.stock > 0 ? `${variant.stock} in stock` : "Out of Stock"})`;
      select.appendChild(option);
    });
    select.addEventListener("change", e => {
      card.dataset.selectedVariantId = e.target.value;
      updateAddToCartButton(card, product);
    });
    variantDiv.appendChild(select);
  } else {
    variantDiv.textContent = `Variant: ${product.variants[0].name}`;
  }

  const addBtn = document.createElement("button");
  addBtn.className = "add-to-cart-button";
  addBtn.textContent = "Add to Cart";
  details.appendChild(addBtn);

  updateAddToCartButton(card, product);

  addBtn.addEventListener("click", () => {
    const variantId = card.dataset.selectedVariantId;
    const variant = product.variants.find(v => v.id === variantId);
    if (!variant || variant.stock === 0) return alert("Out of Stock");

    const existing = cart.find(i => i.productId === product.id && i.variantId === variantId);
    if (existing) {
      existing.quantity++;
    } else {
      cart.push({
        productId: product.id,
        name: product.name,
        variantId,
        variantName: variant.name,
        price: product.price,
        quantity: 1
      });
    }
    renderCart();
  });

  return card;
}

function renderCart() {
  const container = document.getElementById("cart-items");
  const totalEl = document.getElementById("cart-total");
  container.innerHTML = "";

  let total = 0;
  if (cart.length === 0) {
    container.innerHTML = `<p class="text-sm text-gray-500">Your cart is empty.</p>`;
  }

  cart.forEach((item, index) => {
    const div = document.createElement("div");
    div.className = "flex justify-between items-start mb-3";
    div.innerHTML = `
      <div>
        <p class="font-medium">${item.name}</p>
        <p class="text-sm text-gray-600">${item.variantName} x ${item.quantity}</p>
        <p class="text-sm text-gray-800">$${(item.price * item.quantity).toFixed(2)}</p>
      </div>
    `;

    const removeBtn = document.createElement("button");
    removeBtn.textContent = "Remove";
    removeBtn.className = "text-red-600 text-sm font-semibold";
    removeBtn.onclick = () => {
      cart.splice(index, 1);
      renderCart();
    };

    div.appendChild(removeBtn);
    container.appendChild(div);
    total += item.price * item.quantity;
  });

  totalEl.textContent = total.toFixed(2);
}

function renderProductCarousel(parent, products) {
  const wrapper = document.createElement("div");
  wrapper.className = "product-carousel-wrapper";
  parent.appendChild(wrapper);

  const title = document.createElement("h2");
  title.className = "carousel-title";
  title.textContent = "Featured Products";
  wrapper.appendChild(title);

  const trackWrap = document.createElement("div");
  trackWrap.className = "carousel-content-wrapper";
  wrapper.appendChild(trackWrap);

  const prevBtn = document.createElement("button");
  prevBtn.className = "carousel-button prev";
  prevBtn.innerHTML = "&larr;";
  trackWrap.appendChild(prevBtn);

  const container = document.createElement("div");
  container.className = "carousel-container";
  trackWrap.appendChild(container);

  carouselTrack = document.createElement("div");
  carouselTrack.className = "carousel-track";
  container.appendChild(carouselTrack);

  products.forEach(p => {
    const card = createProductCard(p);
    carouselTrack.appendChild(card);
  });

  const nextBtn = document.createElement("button");
  nextBtn.className = "carousel-button next";
  nextBtn.innerHTML = "&rarr;";
  trackWrap.appendChild(nextBtn);

  function updateCarousel() {
    const maxIndex = Math.max(0, products.length - visibleCards);
    prevBtn.disabled = currentIndex === 0;
    nextBtn.disabled = currentIndex >= maxIndex;
    carouselTrack.style.transform = `translateX(-${currentIndex * (cardWidth + 16)}px)`;
  }

  function updateDimensions() {
    const width = container.offsetWidth;
    visibleCards = width >= 1024 ? 4 : width >= 768 ? 3 : width >= 640 ? 2 : 1;
    cardWidth = (width - (visibleCards * 16)) / visibleCards;
    carouselTrack.querySelectorAll(".carousel-card-item").forEach(item => {
      item.style.width = `${cardWidth}px`;
    });
    updateCarousel();
  }

  prevBtn.onclick = () => {
    currentIndex = Math.max(0, currentIndex - visibleCards);
    updateCarousel();
  };

  nextBtn.onclick = () => {
    const maxIndex = Math.max(0, products.length - visibleCards);
    currentIndex = Math.min(maxIndex, currentIndex + visibleCards);
    updateCarousel();
  };

  window.addEventListener("resize", updateDimensions);
  updateDimensions();
}

document.addEventListener("DOMContentLoaded", () => {
  renderProductCarousel(document.getElementById("app-root"), products);
  renderCart();
});
