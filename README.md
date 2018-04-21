#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<fcntl.h>
#include<string.h>
#include <io.h>   
#include<conio.h>
struct SOTHICH
{
	wchar_t* ST[100];
	int n;
};
struct SinhVien
{
	wchar_t* MSSV;
	wchar_t* HoTen;
	wchar_t* Khoa;
	int Khoa1;
	wchar_t* NgaySinh;
	wchar_t* HinhAnh;
	wchar_t* MoTa;
	SOTHICH Sothich;

};
int Wlen(wchar_t *s)
{
	int i = 0;
	if (s == NULL) return -1;
	while (*(s + i) != L'\n'&&*(s + i) != '\0')
	{
		i++;
	}
	return i;
}
void TOKen(wchar_t* s, wchar_t* &d, int &post)
{
	int lengh = Wlen(s);
	wchar_t d1[100];
	while (post < lengh)
	{
		if (s[post++] == 8220) break;
	}
	int i = post;
	while (i < lengh&&s[i] != 8221)
	{
		d1[i - post] = s[i];
		i++;
	}
	d1[i - post] = L'\0';
	d = new wchar_t[Wlen(d1)];
	wcscpy(d, d1);
	d[Wlen(d1)] = L'\0';
	post = i+1;
}
int Wchar_int(wchar_t* s)
{
	int k = 0;
	int lengh = Wlen(s);
	for (int i = 0; i < lengh; i++)
	{
		k = k * 10 + (s[i] - 48);
	}
	return k;
}
wchar_t * TachTen(wchar_t*S)
{
	int k = 0;
	wchar_t d[10];
	d[k++] = S[0];
	for (int i = 1; i < Wlen(S)-1; i++)
	{
		if (S[i] == L' '&&S[i + 1] != L' ') d[k++] = S[i + 1];
	}
	d[k] = L'\0';
	for (int i = 0; i < k; i++)
	{
		if (L'A' <= d[i] && d[i] <= L'Z') d[i] = d[i] + 32;
	}
	wchar_t*s = new wchar_t[k];
	wcscpy(s, d);
	s[k] = '\0';
	return s;
}
void Thong_tin_SV(SinhVien &SV, FILE* FP)
{
	int post = 0;
	wchar_t Thong_tin[1000];
	wchar_t *s;
	fgetws(Thong_tin, 1000, FP);
	TOKen(Thong_tin, SV.MSSV, post);
	TOKen(Thong_tin, SV.HoTen, post);
	TOKen(Thong_tin, SV.Khoa, post);
	TOKen(Thong_tin, s, post);
	SV.Khoa1 = Wchar_int(s);
	TOKen(Thong_tin, SV.NgaySinh, post);
	TOKen(Thong_tin, SV.HinhAnh, post);
	TOKen(Thong_tin, SV.MoTa, post);
	int i = 0;
	while (post < Wlen(Thong_tin))
	{
		TOKen(Thong_tin, SV.Sothich.ST[i++], post);
	}
	SV.Sothich.n = i;
}

//void Xuat_thongTin(SinhVien SV)
//{
//	wchar_t* khoa;
//	fputws(SV.MSSV, stdout); fwprintf(stdout, L"   ");
//	fputws(SV.HoTen, stdout); fwprintf(stdout, L"   ");
//	fputws(SV.Khoa, stdout); fwprintf(stdout, L"   ");
//	fputws(SV.NgaySinh, stdout); fwprintf(stdout, L"   ");
//	fputws(SV.HinhAnh, stdout); fwprintf(stdout, L"   ");
//	fputws(SV.MoTa, stdout); fwprintf(stdout, L"   ");
//	for (int i = 0; i < SV.Sothich.n; i++)
//	{
//		fputws(SV.Sothich.ST[i], stdout); fwprintf(stdout, L"   ");
//	}
//	fwprintf(stdout, L"\n");
//}
wchar_t* TenHTML(wchar_t* s)
{
	wchar_t* S1;
	wchar_t s2[10] = L".html";
	int l = Wlen(s) + 5;
	S1 = new wchar_t[l];
	wcscpy(S1, s);
	for (int i = Wlen(s); i < l; i++)
	{
		S1[i] = s2[i - Wlen(s)];
	}
	S1[l] = L'\0';
	return S1;
}
int main() {
	_setmode(_fileno(stdout), _O_U16TEXT); 
	_setmode(_fileno(stdin), _O_U16TEXT);
	FILE* fin = _wfopen(L"dca.csv", L"r, ccs=UTF-8");
	if (fin == NULL) wprintf(L"\nChuong tr�nh k?t th�c.\n");
	else
	{
		while (!feof(fin))
		{
			SinhVien sv;
			Thong_tin_SV(sv, fin);
			FILE* fin2 = _wfopen(TenHTML(sv.MSSV), L"w, ccs=UTF-8");
			if (fin2 == NULL) wprintf(L"kh�ng m? duoc file");
			else
			{
				fwprintf(fin2, L"<!DOCTYPE html PUBLIC \" -//W3C//DTD XHTML 1.0 Transitional//EN\" \"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd \" >\n");
				fwprintf(fin2, L"<html xmlns=\"http://www.w3.org/1999/xhtml \">\n<head>\n<meta http-equiv=\"Content - Type\" content=\"text/html; charset = utf - 8\" />\n");
				fwprintf(fin2, L"<link rel=\"stylesheet\" type=\"text/css\" href=\"personal.css\" />\n<title>HCMUS - %s</title>\n</head>\n<body>\n<div class=\"Layout_container\">\n", sv.HoTen);
				fwprintf(fin2, L"<!-- Begin Layout Banner Region -->\n<div class=\"Layout_Banner\" >\n<div><img id=\"logo\" src=\"Images/logo.jpg\" height=\"105\" /></div>\n");
				fwprintf(fin2, L"<div class=\"Header_Title\">TRU?NG �?I H?C KHOA H?C T? NHI�N </div>\n</div>\n<!-- End Layout Banner Region -->\n<!-- Begin Layout MainContents Region -->\n<div class=\"Layout_MainContents\">\n");
				fwprintf(fin2, L"<!-- Begin Below Banner Region -->\n<div class=\"Personal_Main_Information\">\n<div class=\"Personal_Location\">\n");
				fwprintf(fin2, L"<img src=\"Images/LogoFooter.jpg\" width=\"27\" height=\"33\" />\n<span class=\"Personal_FullName\">%s - %s</span>\n", sv.HoTen, sv.MSSV);
				fwprintf(fin2, L"<div class=\"Personal_Department\">%s</div>\n<br />\n<div class=\"Personal_Phone\">\nEmail: %s@gmail.com\n", sv.Khoa,TachTen(sv.HoTen));
				fwprintf(fin2, L"</div>\n<br />\n<br />\n</div>\n<div class=\"Personal_HinhcanhanKhung\">\n<img src=\"Images/%s\" class=\"Personal_Hinhcanhan\" />\n", sv.HinhAnh);
				fwprintf(fin2, L"</div>\n</div>\n<!-- End Below Banner Region -->\n<!-- Begin MainContents Region -->\n<div class=\"MainContain\">\n\n<!-- Begin Top Region -->\n<div class=\"MainContain_Top\">\n\n");
				fwprintf(fin2, L"<div class=\"InfoGroup\">Th�ng tin c� nh�n</div>\n<div>\n<ul class=\"TextInList\">\n<li>H? v� t�n: %s</li>\n", sv.HoTen);
				fwprintf(fin2, L"<li>MSSV: %s</li>\n<li>Sinh vi�n khoa %s</li>\n<li>Ng�y sinh: %s</li>\n<li>Email: %s@gmail.com</li>\n</ul>\n", sv.MSSV, sv.Khoa, sv.NgaySinh, TachTen(sv.HoTen));
				fwprintf(fin2, L"</div>\n<div class=\"InfoGroup\">S? th�ch</div>\n<div>\n<ul class=\"TextInList\">");
				for (int i = 0; i < sv.Sothich.n; i++)
				{
					fwprintf(fin2, L"<li>%s", sv.Sothich.ST[i]);
				}
				fwprintf(fin2, L"</ul>");
				fwprintf(fin2, L"</div>\n<div class=\"InfoGroup\">M� t?</div>\n<div class=\"Description\">\n<li>%s.\n\t\t\t\t\t\t</div>\n\n</div>\n</div>\n<d/iv>\n\n<!-- Begin Layout Footer -->\n", sv.MoTa);
				fwprintf(fin2, L"<!-- Begin Layout Footer -->\n<div class=\"Layout_Footer\">\n<div class=\"divCopyright\">\n<br />\n<img src=\"Images/LogoFooter_gray.jpg\" width=\"34\" height=\"41\" /><br />\n<br />\n@2013</br>\nt�? �n gi? k?</br>\n");
				fwprintf(fin2, L"K? thu?t l?p tr�nh</br>\nTH2018/03</br>\n1712901 - Tr?n Ch� Vi </br>\n</div>\n</div>\n<!-- End Layout Footer -->\n</div>\n</body>\n</html>");
				fclose(fin2);
			}
		}
		fclose(fin);
	}
	wprintf(L"\nChuong tr�nh k?t th�c.\n");
	_getch();
	return 0;
}