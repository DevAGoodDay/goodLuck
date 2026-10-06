VersenyAdat.cs
..
using System.Threading.Tasks;
using MySql.Data.MySqlClient;

namespace VersenyGUI
{
    class VersenyAdat
    {
        public string Versenyszam { get; set; }
        public string Nev { get; set; }
        public string Nemzetiseg { get; set; }
        public string Eredmeny { get; set; }

        public VersenyAdat(MySqlDataReader dr)
        {
            Versenyszam = dr.GetString(0);
            Nev = dr.GetString(1);
            Nemzetiseg = dr.GetString(2);
            Eredmeny = dr.GetString(3);
        }

    }
}

MW.xaml.cs

...
using System.Windows.Shapes;
using MySql.Data.MySqlClient;
using VersenyGUI;

namespace AtletikaGUI
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {

        List<VersenyAdat> adatok;
        private readonly string connectionString = "datasource=127.0.0.1;port=3306;username=root;password=;database=verseny;";
        private readonly MySqlConnection connection;


        public MainWindow()
        {
            InitializeComponent();
            connection = new(connectionString);
            connection.Open();
            Beolvas();
        }

        public void Beolvas()
        {
            adatok = [];
            string szoveg = """
                SELECT Versenyszam,VersenyzoNev,Nemzet,Eredmeny
                FROM versenyekszamok INNER JOIN nemzetek on versenyekszamok.NemzetKod=nemzetek.NemzetId
                """;
            MySqlCommand command = new(szoveg, connection);
            MySqlDataReader reader = command.ExecuteReader();
            while (reader.Read())
            {
                adatok.Add(new(reader));
            }
            reader.Close();
            eredmenyTabla.ItemsSource = adatok;
            eredmenyTabla.SelectedIndex = 0;

        }

        private void HelyezesButton_Click(object sender, RoutedEventArgs e)
        {
            VersenyAdat valasztott = eredmenyTabla.SelectedItem as VersenyAdat;

            if (valasztott == null)
                return;

            string szoveg = $"""
                SELECT Helyezes
                FROM versenyekszamok
                WHERE VersenyzoNev='{valasztott.Nev}'
                """;
            MySqlCommand command = new(szoveg, connection);
            MySqlDataReader reader = command.ExecuteReader();

            if (reader.Read())
            {
                helyezesLabel.Content = reader.GetInt32(0);
            }
           
            reader.Close() ;
        }

        private void MagyarHelyezesButton_Click(object sender, RoutedEventArgs e)
        {
            string szoveg = """
                SELECT COUNT(*)
                FROM versenyekszamok inner JOIN nemzetek on
                versenyekszamok.NemzetKod=nemzetek.NemzetId
                WHERE Nemzet="Magyarország" AND Helyezes<=3
                """;
            MySqlCommand command = new(szoveg, connection);
            MySqlDataReader reader = command.ExecuteReader();
            reader.Read();
            magyarHelyezesLabel.Content = $"{reader.GetInt32(0)}";

        }
    }
}

MW.xaml

<Window x:Class="AtletikaGUI.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:AtletikaGUI"
        mc:Ignorable="d"
        Title="AtletikaGUI" Height="450" Width="800">
    <Grid Height="434" VerticalAlignment="Bottom">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="2*"/>
            <ColumnDefinition Width="1*"/>
        </Grid.ColumnDefinitions>
        <Grid Grid.Column="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="1*"/>
                <RowDefinition Height="1*"/>
                <RowDefinition Height="5*"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="1*"/>
                <ColumnDefinition Width="1*"/>
            </Grid.ColumnDefinitions>
            <StackPanel>
                <Button x:Name="HelyezesButton" Content="Helyezés" Click="HelyezesButton_Click"/>
            </StackPanel>
            <StackPanel Grid.Column="1">
                <Label x:Name="helyezesLabel" Content=""/>
            </StackPanel>
            <StackPanel Grid.Row="1">
                <Button x:Name="MagyarHelyezesButton" Content="Magyar helyezés" Click="MagyarHelyezesButton_Click"/>
            </StackPanel>
            <StackPanel Grid.Column="1" Grid.Row="1">
                <Label x:Name="magyarHelyezesLabel" Content=""/>
            </StackPanel>
        </Grid>
        <DataGrid x:Name="eredmenyTabla" SelectionMode="Single" d:ItemsSource="{d:SampleData ItemCount=5}"/>
    </Grid>
</Window>
