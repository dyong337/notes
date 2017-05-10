# Android6.0 权限处理 #
关于Android6.0权限处理的介绍可以看[鸿洋大神的这篇文章](http://blog.csdn.net/lmj623565791/article/details/50709663)  

## Android6.0权限处理实例 - 拨打电话 ##

请求权限  

	//拨打电话
	private void callPhone() {
        //检查权限
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED) {
            //权限未获得

            //用于给用户一个申请权限的解释，该方法只有在用户在上一次已经拒绝过你的这个权限申请。也就是说，用户已经拒绝一次了，你又弹个授权框，你需要给用户一个解释，为什么要授权，则使用该方法。
            if (ActivityCompat.shouldShowRequestPermissionRationale(PermissionTestActivity.this, Manifest.permission.READ_CONTACTS)) {
                // Show an expanation to the user *asynchronously* -- don't block
                // this thread waiting for the user's response! After the user
                // sees the explanation, try again to request the permission.
                callPhone(); //重新请求一次
            } else {
                //请求权限
                ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.CALL_PHONE}, MY_PERMISSIONS_REQUEST_CALL_PHONE);
            }
        } else {
            //权限已获得

            //拨打电话
            Intent intent = new Intent(Intent.ACTION_CALL);
            Uri data = Uri.parse("tel:" + "10010");
            intent.setData(data);
            startActivity(intent);
        }
    }

由于权限的请求是异步的，所以我们要实现权限处理的回调  

	@Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions,int[] grantResults) {

        if (requestCode == MY_PERMISSIONS_REQUEST_CALL_PHONE) {
            if (grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                //如果有多个权限需要授权，会存储在grantResults数组中
                callPhone();
            } else {
                Toast.makeText(PermissionTestActivity.this, "授权被取消", Toast.LENGTH_SHORT).show();
            }
            return;
        }
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    }

## 使用第三方库来处理Android6.0权限 ##
在Github上发现一个叫[RxPermissions](https://github.com/tbruyelle/RxPermissions)的第三方库。  

使用的是RxJava的形式。 

比如上面拨打电话的操作:  

	private void callPhone() {
        RxPermissions.getInstance(this)
                .request(Manifest.permission.CALL_PHONE)
                .subscribe(granted -> {
                    if (granted) { //授权成功                       
                        Intent intent = new Intent(Intent.ACTION_CALL);
                        Uri data = Uri.parse("tel:" + "10010");
                        intent.setData(data);
                        startActivity(intent);
                    } else {
                        Toast.makeText(getApplicationContext(), "权限被取消", Toast.LENGTH_SHORT).show();
                    }
                });
    }  

是不是简单多了 ? 推荐使用 ! 