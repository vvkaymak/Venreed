# Venreed
PLC → C# → SQL → Real-Time Monitoring Pipeline
    // -------------- SQL KAYIT --------------
    if (tetik && !oncekiTetik)
        KaydetVeriSQL(plcName, ints, strs, bitler);

    // -------------- VERİ GÖR --------------
    if (VeriGor[plcIndex])
    {
        VeriGor[plcIndex] = false;

        int[] intsCopy = (int[])ints.Clone();
        string[] strsCopy = (string[])strs.Clone();
        bool[] bitsCopy = (bool[])bitler.Clone();

        this.BeginInvoke(new Action(() =>
        {
            new VeriForm(plcName, tetik, intsCopy, strsCopy,bitsCopy).ShowDialog();
        }));
    }

    oncekiTetik = tetik;

    // -------------- UI - Cycle Time --------------
    long cycle = plcSw[plcIndex].ElapsedMilliseconds;
    this.BeginInvoke(new Action(() =>
    {
        plcCycleLabels[plcIndex].Text = $"{cycle} ms";
    }));
}
catch
{
    connected = false;

    this.BeginInvoke(new Action(() =>
    {
        plcStatusLabels[plcIndex].Text = "Bağlı Değil";
        plcStatusLabels[plcIndex].ForeColor = Color.Red;
    }));
}
