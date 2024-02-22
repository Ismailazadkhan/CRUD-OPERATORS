document.querySelector("#ProductForm")
    .addEventListener('submit', async (event) => {
        event.preventDefault();

        const rm code = document.querySelector('#rm code').value;
        const brand = document.querySelector('#brand').value;
        const supplier = document.querySelector('#supplier').value;
        const price = document.querySelector('#price').value;
        const quantity = document.querySelector('#quantity').value;

        try {
            const resp = await axios.post(`http://localhost:3000/product`, {
                rm code: rm code,
                brand,
                supplier,
                price,
                quantity
            });
            console.log("resp: ", resp.data);
            getAllProducts();

            document.querySelector('#rm code').value = '';
            document.querySelector('#brand').value = '';
            document.querySelector('#supplier').value = '';
            document.querySelector('#price').value = '';
            document.querySelector('#quantity').value = '';

        } catch (e) {
            console.error("Error getting products");
        }

    })


const getAllProducts = async () => {
    try {

        const resp = await axios.get("http://localhost:3000/products");
        console.log("resp: ", resp.data.data);

        let productsDiv = document.querySelector("#products")
        productsDiv.innerHTML = "";

        resp.data.data.map(eachProduct => {
            productsDiv.innerHTML += `<div class="card">
                    <h2>Name: ${eachProduct.name}</h2>
                    <p><strong>Rm Code:</strong> ${eachProduct.rm code}</p>
                    <p><strong>Brand:</strong> ${eachProduct.brand}</p>
                    <p><strong>Supplier:</strong> ${eachProduct.supplier}</p>
                    <p><strong>Price:</strong> ${eachProduct.price} $</p>
                    <p><strong>Pro.id:</strong> ${eachProduct._id}</p>
                    <p><strong>Quantity:</strong> ${eachProduct.quantity}</p>
                    <br>
                    <button class="icon-button delete-button"type="button"
                    onclick="deleteProduct('${eachProduct._id}') ">
                    <i class="fas fa-trash"></i> Delete
                    </button>
                   
                    <button class="icon-button edit-button"type="button"
                    onclick='editProduct(${JSON.stringify(eachProduct)})'>
                    <i class="fas fa-pencil-alt"></i> Edit
                    </button>
                    
                    <hr>
                    <div id="box_${eachProduct._id}"></div>

                </div>`
        })
    } catch (e) {
        console.error("Error getting products");
    }
};
window.addEventListener("load", getAllProducts);


window.editProduct = async (product) => {

    console.log("product: ", product);

    let box = document.querySelector(`#box_${product._id}`);
    box.innerHTML = '';

    box.innerHTML = `<form onsubmit="updateProduct(event, '${product._id}')">
    
        Rm code: <input required type="text" id="rm code_${product._id}" value='${product.rm code}'>
        <br>
        Brand: <input required type="text" id="brand_${product._id}" value='${product.brand}'>
        <br>
        Supplier: <input required type="text" id="supplier_${product._id}" value='${product.supplier}'>
        <br>
        Price: <input required type="number" id="price_${product._id}" value='${product.price}'>
        <br>
        Quantity: <input required maxlength="200" type="text" id="quantity_${product._id}" 
        value='${product.quantity}'>
        <br>
            <button class="update-button" type="submit">
                <i class="fas fa-check"></i> Update
            </button>
            <button class="cancel-button" type="button" onclick="cancelEdit('${product._id}')">
                <i class="fas fa-times"></i> Cancel
            </button>
        </form>
        `
};
window.updateProduct = async (event, id) => {
    event.preventDefault();
    console.log("id: ", id);

    const rm code = document.querySelector(`#rm code_${id}`).value;
    const brand = document.querySelector(`#brand_${id}`).value;
    const supplier = document.querySelector(`#supplier_${id}`).value;
    const price = document.querySelector(`#price_${id}`).value;
    const quantity = document.querySelector(`#quantity_${id}`).value;

    try {
        const resp = await axios.put(`http://localhost:3000/product/${id}`, {
            rm code, brand, supplier, price, quantity
        });
        console.log("resp: ", resp.data);
        getAllProducts();

    } catch (e) {
        console.error("Error getting products");
    }
};
window.cancelEdit = (productId) => {
    const editBox = document.querySelector(`#box_${productId}`);
    editBox.innerHTML = '';
};


window.deleteProduct = async (id) => {
    try {
        console.log("id: ", id);
        const resp = await axios.delete(`http://localhost:3000/product/${id}`);
        console.log("resp: ", resp.data);
        getAllProducts();

    } catch (e) {
        console.error("Error getting products");
    }
};
