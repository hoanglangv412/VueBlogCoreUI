<template>
  <div class="TheAccount">
    <CCard>
      <CCardHeader>
        <h3>Accounts management</h3>
      </CCardHeader>
      <CCardBody class="text">
        <CButton color="info" class="m-2 btn-add float-right mr-5" @click="CreateEditModal = true">
          <!-- <nuxt-link to="/account/add" class="text-white d-block">Add +</nuxt-link> -->Add +
        </CButton>
        <CModal title="Modal title" color="info" :show.sync="CreateEditModal">
          Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
          tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
          quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
          consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
          cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
          proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </CModal>
        <CDataTable :items="dataAccounts" :fields="account" :items-per-page="5" hover pagination>
          <template #BlogStatus="{ item }">
            <td>
              <CBadge :color="getBadge(item.BlogStatus)">
                {{ getName(item.BlogStatus) }}
              </CBadge>
            </td>
          </template>
          <template #method="{ item }">
            <td class="py-2">
              <CButton color="warning" class="btn-update">
                <nuxt-link :to="`/account/add/${item.BlogID}`">
                  <CIcon :content="$options.freeSet.cilPencil"/>
                </nuxt-link>
              </CButton>
              <CButton color="danger" @click="deleteAccount(item.BlogID)"  class="btn-delete">
                  <CIcon :content="$options.freeSet.cilTrash"/>
              </CButton>
            </td>
          </template>
        </CDataTable>
      </CCardBody>
    </CCard>
  </div>
</template>
<script>
import axios from 'axios';
import {freeSet} from '@coreui/icons';
import swal from "sweetalert2";

const accountItem = [
  { key: "BlogID", label: "ID", _style: "min-width:100px" },
  { key: "BlogName", label: "Name", _style: "min-width:150px" },
  { key: "BlogType", label: "Type", _style: "min-width:100px;" },
  { key: "BlogStatus", label: "Status", _style: "min-width:100px;" },
  { key: "BlogAddress", label: "Address", _style: "min-width:100px;" },
  { key: "BlogPostedDate", label: "PostedDate", _style: "min-width:100px" },
  { key: "BlogShortDetail", label: "ShortDetail", _style: "min-width:150px" },
  { key: "BlogDetail", label: "Detail", _style: "min-width:100px;" },
  { key: "BlogPhoto", label: "Photo", _style: "min-width:100px;" },
  { key: "method", label: "Method", _style: "min-width:100px;" },
];
export default {
  name: "TheAccount",
  freeSet,
  props:{
    dataAccounts:{
      type:Array,
      default:()=>[]
    }
  },
  data(){
    return{
      account: accountItem,
      infoModal: false
    };
  },
  methods:{
    deleteAccount(ID){
      swal
      .fire({
        title:"Are you sure?",
        text:"This account will be deleted instantly.",
        icon:"warning",
        showCancelButton:true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Yes, delete it!",
      })
      .then((result) =>{
        if(result.isConfirmed){
          axios.delete("https://localhost:44315/api/Blog/" + ID)
          .then((res) =>{
            this.$emit("getListAccounts",this.dataAccounts);
          });
          swal.fire("Deleted!","This account has been deleted.","success");
        }
      });
    },
    getBadge (BlogStatus) {
      return BlogStatus == '1' ? 'success'
        : BlogStatus == '0' ? 'secondary' : ''
    },
    getName (BlogStatus){
      return BlogStatus == '1' ? 'Active'
        : BlogStatus == '0' ? 'Inactive' : ''
    }
  }
};
</script>