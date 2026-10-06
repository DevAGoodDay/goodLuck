Class.cs

namespace Verseny
{
    internal class Versenyzo
    {
        public Versenyzo(string versenyszam, string versenyzoNev, char nem, string nemzet, string eredmeny, int helyezes, string csucs)
        {
            Versenyszam = versenyszam;
            VersenyzoNev = versenyzoNev;
            Nem = nem;
            Nemzet = nemzet;
            Eredmeny = eredmeny;
            Helyezes = helyezes;
            Csucs = csucs;
        }

        public string Versenyszam {  get; set; }
        public string VersenyzoNev {  get; set; }
        public char Nem {  get; set; }
        public string Nemzet { get; set; }
        public string Eredmeny { get; set; }
        public int Helyezes { get; set; }
        public string Csucs { get; set; }

        public bool Dij()
        {
            return Nemzet == "Németország" && Helyezes <= 3;
        } 
    }
}

Feladat.cs

namespace Verseny
{
    internal class Feladat
    {
        List<Versenyzo> adatok = [];
        public Feladat()
        {
            foreach (var item in File.ReadAllLines("verseny.csv", Encoding.UTF8).Skip(1))
            {
                string[] resz = item.Split(';');
                string versenyszam = resz[0];
                string versenyzoNev = resz[1];
                char nem = Convert.ToChar(resz[2]);
                string nemzet = resz[3];
                string eredmeny = resz[4];
                int helyezes = Convert.ToInt32(resz[5]);
                string csucs = resz[6];
                adatok.Add(new(versenyszam, versenyzoNev, nem, nemzet, eredmeny, helyezes, csucs));
            }
        }

        public void Feladat4()
        {
            Console.WriteLine($"4. Feladat: A versenyzők száma {adatok.Count} fő.");
        }

        public void Feladat5()
        {
            Console.WriteLine("5. Feladat: Az egyéni csúcsot elért versenyzők:");
            var eredmeny = adatok.Where(x => x.Csucs == "SB");
            foreach (var item in eredmeny)
            {
                Console.WriteLine(item.VersenyzoNev);
            }

        }

        public void Feladat7()
        {
            Console.Write("7. Feladat: Kérem a csúcs azonosítóját: ");
            string csucs = Console.ReadLine();
            var eredmeny = adatok.Where(x => x.Csucs == csucs);
            if (eredmeny.Count() == 0)
            {
                Console.WriteLine("Nem található ilyen azonosító!");
            }
            else
            {
                double atlag = eredmeny.Average(x => x.Helyezes);
                Console.WriteLine($"A versenyzők átlagos helyezése: {atlag:n2}");
            }
        }
    }
}

Program.cs

namespace Verseny
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Feladat f = new();
            f.Feladat4();
            f.Feladat5();
            f.Feladat7();
        }
    }
}