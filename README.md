# AndroidEncyclopedia
AndroidEncyclopedia by qacorn



    @GET
    @Streaming
    Observable<ResponseBody> downloadShapeFile(@Url String downloadUrl);


 private void downloadShapeFile(String url) {
        LivableMapNetworkApi.getService(LoginService.class)
                .downloadShapeFile(url)
                .subscribeOn(Schedulers.io())
                .observeOn(Schedulers.newThread())
                .subscribe(new DisposableObserver<ResponseBody>() {
                    @Override
                    public void onNext(@io.reactivex.rxjava3.annotations.NonNull ResponseBody responseBody) {
                        String path = getCacheDir().getAbsolutePath() + File.separator + "/fullShp/shapefile.zip";

                        String unZipPath = getCacheDir().getAbsolutePath();
                        try {

                            File file = new File(path);
                            InputStream is = responseBody.byteStream();
                            FileOutputStream fos = new FileOutputStream(file);
                            BufferedInputStream bis = new BufferedInputStream(is);
                            byte[] buffer = new byte[1024];
                            int len;
                            while ((len = bis.read(buffer)) != -1) {
                                fos.write(buffer, 0, len);
                                fos.flush();
                            }
                            fos.close();
                            bis.close();
                            is.close();

                            ZipUtils.UnZipFolder(path, unZipPath);

                        } catch (IOException e) {
                            e.printStackTrace();
                        } catch (Exception e) {
                            e.printStackTrace();
                        }


                    }

                    @Override
                    public void onError(@io.reactivex.rxjava3.annotations.NonNull Throwable e) {

                    }

                    @Override
                    public void onComplete() {

                    }
                });
    }
