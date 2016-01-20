---
layout: post
title: "الگوریتم فلوید-وارشال"
date: 2015-08-04 03:21:35
image: '/assets/img/'
description: 'توضیحات خود را در این مکان وارد کنید'
tags:
- مهندسی نرم افزار پیشرفته
- فلوید
categories:
- الگوریتم های حریصانه
twitter_text: 'توضیحات توییتر خود را در اینجا قرار دهید'
---
فرض کنید یک گراف جهت‌دار وزن‌دار با مجموعه رئوس {1,2,...,n}{1,2,...,n} داریم. اگر گراف بدون جهت بود، می‌توان از هر یال، ۲ یال جهت‌دار ساخت و الگوریتم را اجرا کرد. وزن یال‌های گراف می‌تواند مثبت یا منفی باشد، اما گراف نباید دور با مجموع وزن منفی داشته باشد.

وزن یک مسیر را، مجموع وزن یال‌های آن مسیر می‌نامیم. فاصله‌ی بین دو رأس را وزن کوتاه‌ترین (کم‌وزن‌ترین) مسیر بین آن‌ها در نظر می‌گیریم. می‌خواهیم به ازای هر دو رأس از این گراف، فاصله‌ی بین آن دو رأس را پیدا کنیم. الگوریتم فلوید-وارشال، راه حلی برای این مسئله ارائه می‌کند.

الگوریتم

فرض کنید 1≤i,j,k≤n1≤i,j,k≤n باشد. تمام مسیر‌هایی از رأس ii به رأس jj در نظر بگیرید که رأس‌های میانی مسیر، فقط بتوانند از رأس‌های {1,2,...,k}{1,2,...,k} باشند. مقدار dist(i,j,k)dist(i,j,k) را وزن کوتاه‌ترین (کم‌وزن‌ترین) مسیر در بین این مسیرها می‌گوییم. می‌خواهیم مقادیر distdist را به ازای i,j,ki,j,k های مختلف پیدا کنیم. برای k=0k=0 می‌دانیم:
dist(i,i,0)=0
dist(i,i,0)=0
و اگر ii به jj یال با وزن ww داشته باشد:
dist(i,j,0)=w
dist(i,j,0)=w
و اگر ii به jj یال نداشته باشد:
dist(i,j,0)=∞
dist(i,j,0)=∞
حال مقادیر distdist را به ازای k≥1k≥1 حساب می‌کنیم. فرض کنید به ازای یک kk، تمام مقادیر dist(i,j,k)dist(i,j,k) را به ازای i,ji,j های مختلف، می‌دانیم. با این فرض، می‌خواهیم تمام مقادیر dist(i,j,k+1)dist(i,j,k+1) را به ازای i,ji,j های مختلف بیابیم.

دو رأس a,ba,b را در نظر بگیرید. می‌خواهیم dist(a,b,k+1)dist(a,b,k+1) را بیابیم. مسیرهایی که باید برای محاسبه‌ی dist(a,b,k+1)dist(a,b,k+1) در نظر گرفته شوند، دو حالت دارند؛ یا شامل رأس میانی k+1k+1 نیستند یا شامل رأس میانی k+1k+1 هستند. کم‌وزن‌ترین مسیری که شامل رأس میانی k+1k+1 نیست، وزن dist(a,b,k)dist(a,b,k) را دارد و کم‌وزن‌ترین مسیری که شامل رأس میانی k+1k+1 است، وزن dist(a,k+1,k)+dist(k+1,b,k)dist(a,k+1,k)+dist(k+1,b,k) را دارد (زیرا باید ابتدا از aa به k+1k+1 برویم و سپس به bb برویم. پس:
dist(a,b,k+1)=min(dist(a,b,k),dist(a,k+1,k)+dist(k+1,b,k))
dist(a,b,k+1)=min(dist(a,b,k),dist(a,k+1,k)+dist(k+1,b,k))
یک مثال

در گراف زیر، روند اجرای این الگوریتم را می‌توانید مشاهده کنید: 

پیچیدگی‌ الگوریتم

به ازای هر kk باید روند بالا را طی کنیم. به ازای هر kk نیز باید dist بین هر 22 رأس را محسابه کنیم. پس پیچیدگی زمانی برنامه از O(n×n2)=O(n3)O(n×n2)=O(n3) است. برای حافظه نیز یک آرایه‌ی ۳ بعدی از O(n3)O(n3) نیاز داریم. پس حافظه نیز از O(n3)O(n3) است. هر چند جلوتر راه‌کاری برای کم‌کردن حافظه ارائه می‌شود.

پیاده‌سازی اولیه

شبه کد:

let dist be a 3D array of minimum distances initialized to ∞
for i from 1 to n
	dist[i][i][0] = 0;
for each edge from vertex a to vertex b
	dist[a][b][0] = w //w is the weight of uv edge

for k from 1 to n
	for i from 1 to n
		for j from 1 to n
			dist[i][j][k] = min(dist[i][j][k-1], dist[i][k][k-1] + dist[k][j][k-1]
کاهش حافظه

با کمی دقت در می‌یابیم اگر بعد سوم حافظه را نیز در نظر نگیریم، برنامه به درستی کار می‌کند. پس حافظه را می‌توان از O(n2)O(n2) در نظر گرفت.

پیاده‌سازی

Floyd.cpp
#include <iostream> 
#include <algorithm>
using namespace std; 
 
const int MAXN = 10 * 1000 + 10;
const int oo = 1000 * 1000 * 1000 + 10;
 
int n, m;
int dist[MAXN][MAXN];
 
void inputEdges();
void floydWarshall();
void output();
 
int main() {
	cin >> n >> m;
	for (int i = 0; i < n; i++) {
		fill(dist[i], dist[i] + n, oo);
		dist[i][i] = 0;
	}
	inputEdges();
	floydWarshall();
	output();
}
 
void inputEdges() {
	for (int i = 0; i < m; i++) {
		int u, v, w;
		cin >> u >> v >> w;
		u--, v--;
		dist[u][v] = min(dist[u][v], w);
	}
}
 
void floydWarshall() {
	for (int k = 1; k <= n; k++)
		for (int i = 0; i < n; i++)
			for (int j = 0; j < n; j++)
				dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
}
 
void output() {
	for (int i = 0; i < n; i++)
		for (int j = 0; j < n; j++)
			cout << "distance from vertex " << i + 1 << " to vertex " << j + 1 << " is " << dist[i][j] << '\n';
}
