# upload
练习：文件上传到服务器的练习代码

1.创建磁盘文件项工厂
2.创建核心解析类
3.解析请求对象,将请求分成几个部分(FileItem)
4.遍历集合获得每个部分的对象


文件上传测试
	引入jar包
		commons-fileupload-1.2.1.jar
		commons-io-1.4.jar
		
		// 1.创建磁盘文件项工厂
		DiskFileItemFactory diskFileItemFactory = new DiskFileItemFactory();
		// 2.创建核心解析类
		ServletFileUpload fileUpload = new ServletFileUpload(diskFileItemFactory);
		// 3.解析请求对象,将请求分成几个部分(FileItem)
		try {
			//会返回一个List对象
			List<FileItem> list= fileUpload.parseRequest(request);
			// 4.遍历集合获得每个部分的对象
			for(FileItem fileItem:list){
				// 判断是普通项还是文件上传项
				if(fileItem.isFormField()){
					// 普通项
					// 获得普通项的名称:
					String name = fileItem.getFieldName();
					// 获得普通项的值:
					String value = fileItem.getString("UTF-8");
					System.out.println(name+"  "+value);
				}else{
					// 文件上传项
					// 获得文件的名称:
					String fileName = fileItem.getName();
					// 获得文件的输入流:
					InputStream is = fileItem.getInputStream();
					// 需要将文件写入到服务器的某个路径即可:
					String path = getServletContext().getRealPath("/upload");
					System.out.println(path);
					// 创建输出流 与 输入流进行对接:
					OutputStream os = new FileOutputStream(path+"//"+fileName);
					int len = 0;
					byte[] b = new byte[1024];
					while((len = is.read(b))!=-1){
						os.write(b, 0, len);
					}
					is.close();
					os.close();
				}
			}
		} catch (FileUploadException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	
	}