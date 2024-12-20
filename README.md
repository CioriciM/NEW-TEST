#include <iostream>
#include "BaseClass.h"
#include <string>
#include <cmath>
using namespace std;

enum TipCursValutar {
	cumparare = 10,
	vanzare = 20

};

//Puteti ignora derivarea (ajuta doar la testarea automata)
class CursValutar : public BaseClass
{

private:

	string denumireValuta;
	TipCursValutar tipCurs = vanzare;
	int nrZile;
	float* istoric;
	static string denumireCasaDeSchimb;
	const int idValuta;

	CursValutar() :idValuta(0)
	{
		this->denumireValuta = "N/A";
		this->nrZile = 0;
		this->istoric = nullptr;
		this->tipCurs = vanzare;
	}

	CursValutar(int idValuta, string denumire, string tipCurs) :idValuta(idValuta)
	{
		this->denumireValuta = denumire;
		this->nrZile = 1;
		this->istoric = new float[1];
		this->istoric[0] = 1.0;
		if (tipCurs == "cumparare") {
			this->tipCurs = cumparare;
		}
		else if (tipCurs == "vanzare") {
			this->tipCurs = vanzare;
		}
		else {
			throw invalid_argument("Tipul de curs este invalid");
		}

	}
	CursValutar(const CursValutar& curs) :idValuta(curs.idValuta){
		this->denumireValuta = curs.denumireValuta;
		this->tipCurs = curs.tipCurs;
		this->nrZile = curs.nrZile;

		if (curs.istoric != nullptr) {
			this->istoric = new float[this->nrZile];
			for (int i = 0; i < nrZile; i++) {
				this->istoric[i] = curs.istoric[i];
			}
		}
		else {
			this->istoric = nullptr;
		}


		


	}
	~CursValutar() {
		if (istoric) {
			delete[] istoric;
		}
	}

	CursValutar& operator=(const CursValutar& curs) {
		if (this != &curs) {
			delete[] istoric;

			denumireValuta = curs.denumireValuta;
			tipCurs = curs.tipCurs;
			nrZile = curs.nrZile;

			if (curs.istoric) {
				istoric = new float[nrZile];
				for (int i = 0; i < nrZile; i++) {
					istoric[i] = curs.istoric[i];
				}
			}
			else {
				istoric = nullptr;
			}
		}
		return *this;
	}

	int getNrZile() const
	{
		return this->nrZile;
	}

	float* getIstoric() const {
		if (!istoric) {
			return nullptr;
		}
		float* copy = new float[nrZile];
		for (int i = 0; i < nrZile; i++) {
			copy[i] = istoric[i];
		}
		return copy;
	}

	static string getDenumireCasaDeSchimb() {
		return denumireCasaDeSchimb;
	}
	int getIdValuta() const {
		return idValuta;
	}

	

	void setIstoric(const float* noulIstoric, int nouNrZile) {
		delete[] istoric;

		nrZile = nouNrZile;

		if (nouNrZile > 0 && noulIstoric != nullptr)
		{
			istoric = new float[nouNrZile];
			for (int i = 0; i < nouNrZile; i++) {
				istoric[i] = noulIstoric[i];
			}
		}
		else {
			istoric = nullptr;
			nouNrZile = 0;
		}
	}

	float rataDeSchimb(const string& tipCursStr) const {
		TipCursValutar queryTip = (tipCursStr == "cumparare") ? cumparare : vanzare;

		if (tipCurs != queryTip || !istoric || nrZile == 0)
			return 0.0f;

		return istoric[nrZile - 1];
	}

	// Metodă statică valutaCelMaiMicCurs
	static string valutaCelMaiMicCurs(CursValutar* vector, int nrElemente) {
		if (!vector || nrElemente <= 0)
			return "N/A";

		string minValuta = vector[0].denumireValuta;
		float minCurs = vector[0].istoric[vector[0].nrZile - 1];

		for (int i = 1; i < nrElemente; i++) {
			float currentCurs = vector[i].istoric[vector[i].nrZile - 1];
			if (currentCurs < minCurs) {
				minCurs = currentCurs;
				minValuta = vector[i].denumireValuta;
			}
		}
		return minValuta;
	}

	// Operator +=
	CursValutar& operator+=(float valoare) {
		float* nouIstoric = new float[nrZile + 1];
		for (int i = 0; i < nrZile; i++) {
			nouIstoric[i] = istoric[i];
		}
		nouIstoric[nrZile] = valoare;

		delete[] istoric;
		istoric = nouIstoric;
		nrZile++;
		return *this;
	}

	// Operator []
	float operator[](int index) const {
		if (index < 0 || index >= nrZile)
			return -1.0f;
		return istoric[index];
	}



};
string CursValutar::denumireCasaDeSchimb = "ING";

int main()
{
	//Playgroud
	//Testati aici clasele, metodele, functiile dorite si folositi debugger-ul
	//pentru a rezolva eventualele probleme
}
