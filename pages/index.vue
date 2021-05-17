<template>
  <div class="container" align="center">
    <CBox
      v-bind="mainStyles[colorMode]"
      
      w="100vw"
      h="100vh"
      justify-content="center"
    >
      <CBox justify-content="right" mt="30px" >
        <CIconButton 
          :icon="colorMode === 'light' ? 'moon' : 'sun'"
          :aria-label="`Switch to ${
            colorMode === 'light' ? 'dark' : 'light'
          } mode`"
          @click="toggleColorMode"
        /> Dark Mode
      </CBox>
      <CBox
        justify-content="center"
        w="40%"
        h="70%"
        justify="center"
        mt="10px"
        border="2px"
        borderRadius="lg"
        borderColor="gray.400"
        pb="20px"
      >
        <CHeading text-align="center" mb="4"> Calculator </CHeading>
        <c-text></c-text>
        <c-grid w="80%" color>
          <c-stack spacing="3" is-inline>
            <c-input
              size="md"
              focus-border-color="pink.400"
              type="number"
              v-model="angka1"
            />
            <c-checkbox size="lg" @change="cbAngka1 = !cbAngka1"
            variant-color="red"></c-checkbox>
          </c-stack>
          <br />
          <c-stack spacing="3" is-inline>
            <c-input
              size="md"
              focus-border-color="pink.400"
              type="number"
              v-model="angka2"
            />
            <c-checkbox size="lg" @change="cbAngka2 = !cbAngka2"
            variant-color="red"></c-checkbox>
          </c-stack>
          <br />
          <c-stack spacing="3" is-inline>
            <c-input
              size="md"
              focus-border-color="pink.400"
              type="number"
              v-model="angka3"
            />
            <c-checkbox size="lg" @change="cbAngka3 = !cbAngka3"
            variant-color="red"></c-checkbox>
          </c-stack>
          <br />
          <c-stack spacing="5" is-inline>
            <c-icon-button
              variant="outline"
              variant-color="red"
              aria-label="Drink coffee"
              icon="add"
              @click="operate('addition')"
            />
            <c-icon-button
              variant="outline"
              variant-color="vue"
              aria-label="Send email"
              icon="minus"
              @click="operate('substraction')"
            />
            <c-icon-button
              variant="outline"
              variant-color="red"
             
              icon="close"
              @click="operate('multiply')"
            />
            <c-icon-button
              variant="outline"
              variant-color="vue"
              aria-label="Send email"
              icon="divide"
              @click="operate('division')"
            ></c-icon-button>
          </c-stack>
          <br />
          <c-divider />
          <br />
          <c-stack spacing="5" is-inline>
            <c-text fontSize="2xl" align="left">Hasil: </c-text>
            <c-input
              size="md"
              focus-border-color="pink.400"
              type="number"
              v-model="hasil"
              isReadOnly
            />
          </c-stack>
        </c-grid>
      </CBox>
    </CBox>
  </div>
</template>

<script lang="js">
import {
  CBox,
  CButton,
  CGrid,
  CIconButton,
  CFlex,
  CHeading,CDivider,
  CText,
} from '@chakra-ui/vue'

export default {
  name: 'App',
  components: {
    CBox,
    CButton,
    CGrid,
    
    CIconButton,
    CFlex,
    CHeading, CDivider,
    CText
  },
  inject: ['$chakraColorMode', '$toggleColorMode'],
  data () {
    return {
      showModal: false,
      
      mainStyles: {
        dark: {
          bg: 'gray.700',
          color: 'whiteAlpha.900'
        },
        light: {
          bg: 'white',
          color: 'gray.900'
        }
      },
      angka1:0,
      angka2:0,
      angka3:0,
      hasil:0,
      cbAngka1:false,
      cbAngka2:false,      
      cbAngka3:false,
      errorMessage:"Infinite Result..!!! Divide By Zero, Please Check Your Input",
    }
  },
  computed: {
    colorMode () {
      return this.$chakraColorMode()
    },
    theme () {
      return this.$chakraTheme()
    },
    toggleColorMode () {
      return this.$toggleColorMode
    }
  },
  methods: {
    showInfinity () {
      this.$toast({
        title: 'Something Wrong',
        description: this.errorMessage,
        status: 'warning',
        duration: 2000,
        isClosable: true,
        variant:'subtle'
      })
    },
  
    operate(operator){
      
      if(this.cbAngka1==true && this.cbAngka2==true && this.cbAngka3==true ){
        if(operator=="addition"){
          this.hasil = +this.angka1 + +this.angka2 + +this.angka3;

        }else if(operator=="substraction"){
          
          this.hasil =+this.angka1 - +this.angka2 - +this.angka3;

        }else if(operator=="division"){
           if (this.angka1==0 || this.angka2==0 || this.angka3==0 ){
             this.showInfinity();
           }else{
            this.hasil =+this.angka1 / +this.angka2 / +this.angka3;
           }
        }else {
          this.hasil = +this.angka1 * +this.angka2 * +this.angka3;

        }
      }else  if(this.cbAngka1==true && this.cbAngka2==true){
         if(operator=="addition"){
          this.hasil = +this.angka1 + +this.angka2 ;

        }else if(operator=="substraction"){
          this.hasil = +this.angka1 - +this.angka2;

        }else if(operator=="division"){
          if (this.angka1==0 || this.angka2==0){
             this.showInfinity();
          }else{
          this.hasil = +this.angka1 / +this.angka2 ;
          }
        }else {
          this.hasil = +this.angka1 * +this.angka2;

        }

      }else  if(this.cbAngka1==true && this.cbAngka3==true){
        if(operator=="addition"){
          this.hasil = +this.angka1 + +this.angka3 ;

        }else if(operator=="substraction"){
          this.hasil = +this.angka1 - +this.angka3;

        }else if(operator=="division"){
           if (this.angka1==0 || this.angka3==0){
             this.showInfinity();
          }else{
          this.hasil = +this.angka1 / +this.angka3 ;
          }
        }else {
          this.hasil = +this.angka1 * +this.angka3;

        }
      }else  if(this.cbAngka2==true && this.cbAngka3==true){
        if(operator=="addition"){
          this.hasil = +this.angka2 + +this.angka3 ;

        }else if(operator=="substraction"){
          this.hasil = +this.angka2 - +this.angka3;

        }else if(operator=="division"){
           if (this.angka2==0 || this.angka3==0){
             this.showInfinity();
          }else{
          this.hasil = +this.angka2 / +this.angka3 ;
          }
        }else {
          this.hasil = +this.angka2 * +this.angka3;

        }
      }
    },
    
  }
}
</script>
