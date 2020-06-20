<script>
  import { v4 } from "uuid";
  import Noty from "noty";
  import "noty/lib/noty.css";
  import "noty/lib/themes/sunset.css";

  let isEdit = false;

  let products = [
    {
      id: 0,
      name: "Hp Pavilion",
      description: "HP laptop",
      category: "laptop"
    },
    {
      id: 1,
      name: "Mouse raser",
      description: "Gaming mouse",
      category: "peripherials"
    }
  ];

  let product = {
    id: "",
    name: "",
    description: "",
    category: "",
    imageURL: ""
  };

  const addProduct = () => {
    product.id = v4();
    products = products.concat(product);
    console.log(products);
    product = {};
  };

  const onSubmitHandler = e => {
    // e.preventDefault();
    if (!isEdit) {
      addProduct();
    } else {
      updateProduct();
      product = {};
    }
  };

  const updateProduct = () => {
    let updatedProduct = product;
    products = products.map(p => {
      if (updatedProduct.id === p.id) {
        new Noty({
          theme: "sunset",
          type: "success",
          timeout: 3000,
          text: "Product updated successfully"
        }).show();
        p = updatedProduct;
        return p;
      }
      return p;
    });
  };
  const deleteProduct = id => {
    products = products.filter(product => product.id != id);
    new Noty({
      theme: "sunset",
      type: "success",
      timeout: 3000,
      text: "Product deleted successfully"
    }).show();
  };

  const editProduct = productEdit => {
    isEdit = true;
    product = productEdit;
  };
</script>

<style>
  .info {
    /* position: absolute; */
	/* bottom: 0; */
	
	width: 100%;
	background-color: darkgray;
    height: 55px;
    padding: 10px;
  }

 .info p{
	 margin: 0px auto !important;
	 color: #000;
	 width: 20px;
	 text-transform: uppercase;
 }
</style>

<main>

  <div class="container p-3">

    <div class="row">
      <div class="col-md-6">
        {#each products as product}
          <div class="card mb-2">
            <div class="row">
              <div class="col-md-4">
                {#if !product.imageURL}
                  <img
                    src="images/noproduct.jpg"
                    class="img-fluid p-2"
                    alt="" />
                {:else}
                  <img src={product.imageURL} class="img-fluid p-2" alt="" />
                {/if}
              </div>
              <div class="col-md-8 p-3">
                <div class="card-body">

                  <h5>
                    <strong>{product.name}</strong>
                  </h5>
                  <span>
                    <small>{product.category}</small>
                  </span>
                  <p class="card-text">{product.description}</p>
                  <button
                    on:click={deleteProduct(product.id)}
                    class="btn btn-danger">
                    Delete
                  </button>
                  <button
                    class="btn btn-secondary"
                    on:click={editProduct(product)}>
                    Edit
                  </button>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <form on:submit|preventDefault={onSubmitHandler}>

              <div class="form-group">
                <input
                  class="form-control"
                  bind:value={product.name}
                  type="text"
                  id="product-name"
                  placeholder="Product name" />

              </div>

              <div class="form-group">

                <textarea
                  class="form-control"
                  bind:value={product.description}
                  id="product-description"
                  rows="10" />
              </div>

              <div class="form-group">

                <input
                  class="form-control"
                  bind:value={product.imageURL}
                  type="url"
                  id="product-image-url"
                  placeholder="https://tuweb" />

              </div>

              <div class="form-group">

                <select
                  class="form-control"
                  bind:value={product.category}
                  id="category">
                  <option value="laptops">Laptops</option>
                  <option value="perifericos">Perifericos</option>
                  <option value="servers">Servidores</option>
                </select>
              </div>
              <button class="btn btn-secondary">
                {#if !isEdit}Save product{:else}Edit product{/if}
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="info">
		<p >Sveltjs</p> 
  </div>

</main>
