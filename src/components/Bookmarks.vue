<template>
  <div>

  </div>
</template>

<script>

  export default {
    props: {
      setActive: Function,
      remove: Function,
    },
    data: () => ({
      isActive: false
    }),
    methods: {
      toggleList(){
        this.isActive = !this.isActive
      }
    }
  }

</script>

<style scoped lang="scss">
.bookmarks{
  cursor: pointer;
  font-size: 65px;
  line-height: 0;
  color: #ffffff;
  user-select: none;
  text-shadow: 0 0 4px rgba(black, 0.25);
  display: inline;
  position: absolute;
  top: 88px;
  right: 65px;
  font-family: monospace;
  font-weight: 900;
  letter-spacing: -8px;
  z-index: 10;
  &.active{
    color: #2c3e50;
  }
  &-list{
    transform: translateX(100%);
    transition: all 0.2s ease-in-out;

    &.active{
      transform: translateX(0);
    }

    position: absolute;
    right: 0;
    color: #2c3e50;
    background: #fdfdfd;
    padding: 25px;
    height: 100vh;
    width: 30%;
    ul{
      li{
        border: 1px solid #ccc;
        padding: 5px 20px;
        padding-top: 9px;
        text-align: left;
        list-style: none;
        margin-top: 110px;
        user-select: none;

        display: flex;
        justify-content: space-between;
        align-items: center;

        cursor: pointer;
        p{
          font-weight: 600;
        }
        box-shadow: 0 0 10px rgba(black, 0.1);
        transition: all 0.1s linear;
        &:hover{
          background: rgba(#ccc, 0.1);
          box-shadow: none;

        }
        .remove{
          display: block;
          font-family: sans-serif;

          font-size: 22px;
          font-weight: 500;
          cursor: pointer;
          z-index: 10;
          padding: 10px;
          transition: all 0.1s linear;
          &:hover{
            transform: scale(1.3);
            color: orangered;
          }
        }
      }
    }
  }
}
</style>