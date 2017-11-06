# pdf转高清无码图片

## 首先要下载jacob.jar
## 下载Adobe Acrobat DC 主要是通过软件这个来进行转换
            for(int i=0;i < pageNum;i++){
                            //根据页码得到单页PDF
                            Dispatch page = Dispatch.call(pdfObject, "AcquirePage", new Variant(i)).toDispatch();
                            //得到PDF单页大小的Point对象
                            Dispatch pagePoint = Dispatch.call(page, "GetSize").toDispatch();
                            //创建PDF位置对象，为拷贝图片到剪贴板做准备
                            ActiveXComponent pdfRect = new ActiveXComponent("AcroExch.Rect");
                            /*********************************************************/

                            /**要想设置分辨率高的话就需要将pdf的宽和高设置成分倍率/100的值*/

                            /********************************************************/
                            //得到单页PDF的宽
                            int imgWidth = (int) (Dispatch.get(pagePoint, "x").toInt() * 5);
                            //得到单页PDF的高
                            int imgHeight = (int) (Dispatch.get(pagePoint, "y").toInt() * 5);
                            //控制PDF位置对象
                            Dispatch pdfRectDoc = pdfRect.getObject();
                            //设置PDF位置对象的值
                            Dispatch.put(pdfRectDoc, "Left", new Integer(0));
                            Dispatch.put(pdfRectDoc, "Right", new Integer(imgWidth));
                            Dispatch.put(pdfRectDoc, "Top", new Integer(0));
                            Dispatch.put(pdfRectDoc, "Bottom", new Integer(imgHeight));
                            //将设置好位置的PDF拷贝到Windows剪切板，参数：位置对象,宽起点，高起点，分辨率
                            Dispatch.call(page, "CopyToClipboard",new Object[]{pdfRectDoc,0,0,500});
                            Image image = getImageFromClipboard();
                            BufferedImage tag = new BufferedImage(imgWidth, imgHeight, 8);
                            Graphics graphics = tag.getGraphics();
                            graphics.drawImage(image, 0, 0, null);
                            graphics.dispose();
                            //输出图片
                            ImageIO.write(tag, "JPEG", new File(savePath+i+".jpg"));
                        }