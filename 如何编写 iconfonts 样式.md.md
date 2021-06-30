```css

/* 资源预警 icon 图标 */
@font-face {
  font-family: "resourcewarn";
  src: url("../../assets/fonts/resourcewarn/resourcewarn.eot?9xjmnk");
  src: url("../../assets/fonts/resourcewarn/resourcewarn.eot?9xjmnk#iefix")
      format("embedded-opentype"),
    url("../../assets/fonts/resourcewarn/resourcewarn.ttf?9xjmnk")
      format("truetype"),
    url("../../assets/fonts/resourcewarn/resourcewarn.woff?9xjmnk")
      format("woff"),
    url("../../assets/fonts/resourcewarn/resourcewarn.svg?9xjmnk#resourcewarn")
      format("svg");
  font-weight: normal;
  font-style: normal;
  font-display: block;
}

[class^="warn-icon-"],
[class*=" warn-icon-"] {
  /* use !important to prevent issues with browser extensions that change fonts */
  font-family: "resourcewarn" !important;
  speak: never;
  font-style: normal;
  font-weight: normal;
  font-variant: normal;
  text-transform: none;
  line-height: 1;

  /* Better Font Rendering =========== */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.resourcewarn {
  font-family: "resourcewarn", PingFangSC-Light, "helvetica neue",
    "hiragino sans gb", arial, "microsoft yahei ui", "microsoft yahei", simsun,
    sans-serif !important;
  display: inline-block;
  height: 14px;
  width: 14px;
}

.warn-icon_generateFailed:before {
  content: "\e901";
  color: #f66f6a;
}
.warn-icon_generating:before {
  content: "\e902";
  color: #4eaff5;
}
.warn-icon_noProcess:before {
  content: "\e903";
  color: #989898;
}
.warn-icon_pending:before {
  content: "\e904";
  color: #ff9400;
}
.warn-icon_processing:before {
  content: "\e905";
  color: #5e7ce0;
}
.warn-icon_cleared:before {
  content: "\e900";
  color: #5ccbb1;
}

```
