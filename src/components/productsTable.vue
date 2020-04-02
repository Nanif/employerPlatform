<template>
  <div class="wrapper">
    <a id="downloadAnchorElem" style="display:none"></a>
    <button @click="saveData()">שמירה</button>

    <input type="file" id="uploadFile" value="Import" style="display:none" @change="uploadFile()" />
    <br />
    <button @click="chooseFile()" id="uploadFile">טעינת קובץ</button>
    <div class="table">
      <div class="titles">
        <div class="catalog_id_title title">מק''ט</div>
        <div class="desc_title title">תיאור מוצר</div>
        <div class="price_title title">מחיר</div>
        <div class="size_title title">מידה</div>
        <div class="color_title title">צבע</div>
        <div class="remarks_title title">הערות</div>
      </div>
      <div v-for="(cat, idx) in productsList" :key="idx" class="content_wrapper">
        <div v-for="(prod, index) in cat.itemList" :key="index">
          <input type="text" class="item catalog_id" v-model="prod.product_number" />
          <input type="text" class="item desc" v-model="prod.description" />
          <input type="text" class="item price" v-model="prod.price" />
          <input type="text" class="item size" v-model="prod.size" />
          <input type="text" class="item color" v-model="prod.color" />
          <input type="text" class="item remarks" v-model="prod.remarks" />
          <canvas id="canvas" width="5" height="5"></canvas>
          <input type="file" accept="image/*" class="item image" @change="imageToBase64(prod.id)" />
          <font-awesome-icon class="plus_icon" @click="addProduct()" icon="plus-circle" size="2x" />
        </div>
        <button class="add_categor_btn" @click="addCategory()">
          הוספת קטגוריה
          <font-awesome-icon class="plus_icon" @click="addCategory()" icon="plus-circle" size="2x" />
        </button>
      </div>
    </div>
  </div>
</template>



<script>
export default {
  name: "productsTable",
  data() {
    return {
      product: {
        id: 0,
        product_number: "",
        description: "",
        price: "",
        size: "",
        color: "",
        remarks: "",
        image: ""
      },
      category: {
        id: "",
        itemList: []
      },
      productsList: []
    };
  },
  mounted() {
    var newCategory = JSON.parse(JSON.stringify(this.category));
    this.productsList.push(newCategory);
    this.addProduct();
  },
  methods: {
    addCategory() {
      // let newCategory = Object.assign({}, this.category);
      var newCategory = JSON.parse(JSON.stringify(this.category));
      this.productsList.push(newCategory);
      this.addProduct();
    },
    imageToBase64(idx) {
      var canvas = document.getElementById("canvas");
      var dataURL = canvas.toDataURL();
      this.productsList[idx].image = dataURL;
    },
    addProduct() {
      let newProd = Object.assign({}, this.product);
      this.productsList[0].itemList.push(newProd);
    },
    saveData() {
      var dataStr =
        "data:text/json;charset=utf-8," +
        encodeURIComponent(JSON.stringify(this.productsList));
      var dlAnchorElem = document.getElementById("downloadAnchorElem");
      dlAnchorElem.setAttribute("href", dataStr);
      dlAnchorElem.setAttribute("download", "scene.json");
      dlAnchorElem.click();
    },
    orderIds(list) {
      /* eslint-disable */
      list.forEach(({ id }, idx) => {
        return (id = idx);
      });
      this.productsList = [...list];
    },
    chooseFile() {
      let uploadFileElem = document.getElementById("uploadFile");
      uploadFileElem.click();
    },
    uploadFile() {
      let uploadFileElem = document.getElementById("uploadFile");

      var files = uploadFileElem.files;
      console.log(files);
      if (files.length <= 0) {
        return false;
      }
      let vm = this;
      var fr = new FileReader();
      fr.onload = function(e) {
        console.log(e);
        var result = JSON.parse(e.target.result);
        var formatted = JSON.stringify(result, null, 2);

        let list = JSON.parse(formatted);
        let productsList = [...vm.productsList];
        list.forEach(item => {
          productsList.push(item);
        });

        vm.orderIds(productsList);
      };
      fr.readAsText(files.item(0));
    }
  }
};
</script>

<style lang="scss" scoped >
.wrapper {
  .table {
    position: relative;
    .titles {
      display: flex;
      justify-content: space-between;
      flex-direction: row-reverse;
    }
    .title {
      width: 150px;
    }
    .item {
      width: 150px;
    }
    .plus_icon {
      cursor: pointer;
    }
    .content_wrapper {
      position: relative;

      .add_categor_btn {
        position: absolute;
      }
    }
  }
}
</style>



 