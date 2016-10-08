#PullRefresh
[github地址](https://github.com/orchid-ding/PullRefreshDemo.git)
##效果展示
![这里写图片描述](http://img.blog.csdn.net/20161008195408994)


![这里写图片描述](http://img.blog.csdn.net/20161008195423052)


###Usage
####一.layout
```
<!--直接在布局中申明控件-->
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity" >
    <com.example.librery.PullRefresh
        android:id="@+id/listview"
        android:layout_width="match_parent"
        android:layout_height="match_parent" >
    </com.example.librery.PullRefresh>
</RelativeLayout>
```
####二.回调接口

```
	//获取listVIew
	 listview = (PullRefresh) findViewById(R.id.listview);
         adapter = new MyAdapter();
        listview.setAdapter(adapter);
        listview.setRefreshListener(new PullRefresh.OnRefreshListener() {
            @Override
            public void onRefresh() {
            //下拉刷新，在此添加item
            onrefresh();
            }
            @Override
            public void onLoadMore() {
		//加载更多，添加item
		loadmore();
            }
        });
private void onrefresh(){

     new Thread(){
                    public void run() {
                        try {
                            Thread.sleep(2000);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }

                        listDatas.add(0,"我是下拉刷新出来的数据!");

                        runOnUiThread(new Runnable() {

                            @Override
                            public void run() {
                                adapter.notifyDataSetChanged();
                                listview.onRefreshComplete();
                            }
                        });
                    };

                }.start();
}
private void loadmore(){

new Thread(){
                    public void run() {
                        try {
                            Thread.sleep(2000);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }

                        listDatas.add("我是加载更多出来的数据!1");
                        listDatas.add("我是加载更多出来的数据!2");
                        listDatas.add("我是加载更多出来的数据!3");

                        runOnUiThread(new Runnable() {

                            @Override
                            public void run() {
                                adapter.notifyDataSetChanged();
                                listview.onRefreshComplete();
                            }
                        });
                    };

                }.start();
}
```
####三．Adapter

```
class MyAdapter extends BaseAdapter {

        @Override
        public int getCount() {
            return listDatas.size();
        }
        @Override
        public View getView(int position, View convertView, ViewGroup parent) {
            TextView textView = new TextView(parent.getContext());
            textView.setTextSize(18f);
            textView.setText(listDatas.get(position));

            return textView;
        }

        @Override
        public Object getItem(int position) {
            return listDatas.get(position);
        }

        @Override
        public long getItemId(int position) {
            return position;
        }

    }
```
