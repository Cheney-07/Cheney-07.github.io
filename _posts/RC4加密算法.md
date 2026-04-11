1.初始化S和T
	for(int i=0 ; i<256; i++ )
	{
		S[i]=i;
		T[i]=K[i % keylen]
	}
2.初始化排列S
	   j=0；
	   for(i=0;i<256;i++)
	   {
	     j=(j+S[i]+T[i])%256;
	     swap(S[i],S[j]);
	   }
3.产生密钥流，利用密钥流对明文加密
	   int i,j=0;
	   for(int r=0;r<len;r++)
	   {
	   i=(i+1)%256;
	   j=(j+S[i])%256;
	   swap(S[i],S[j]);
	   t=(S[i]+S[j])%256;
	   k[r]=S[t];//密钥流
	   data[r]^=k[r];//加密
	   }