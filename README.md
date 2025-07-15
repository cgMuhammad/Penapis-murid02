import streamlit as st
import pandas as pd
import datetime

st.set_page_config(page_title="Senarai Murid Mengikut Tarikh Lahir & Kelas")
st.title("📋 Penapis Murid Berdasarkan Tarikh Lahir dan Kelas")

# Muat naik fail Excel
uploaded_file = st.file_uploader("Muat naik fail Excel (.xlsx)", type=["xlsx"])

if uploaded_file:
    df = pd.read_excel(uploaded_file)

    # Pastikan kolum betul
    required_columns = {"Nama", "Kelas", "Tarikh_Lahir"}
    if not required_columns.issubset(set(df.columns)):
        st.error("Fail mesti mengandungi kolum: Nama, Kelas, Tarikh_Lahir")
    else:
        # Tukar kolum tarikh kepada format datetime
        df["Tarikh_Lahir"] = pd.to_datetime(df["Tarikh_Lahir"], errors='coerce')
        df = df.dropna(subset=["Tarikh_Lahir"])  # Buang data tanpa tarikh sah

        # Sidebar untuk penapisan
        st.sidebar.header("🔍 Penapis")

        # Pilih kelas
        kelas_terpilih = st.sidebar.multiselect("Pilih Kelas", sorted(df["Kelas"].unique()))

        # Pilih bulan lahir
        bulan = st.sidebar.selectbox("Pilih Bulan Lahir", ["Semua"] + list(range(1, 13)), index=0)

        # Pilih umur
        umur = st.sidebar.slider("Umur (tahun)", min_value=6, max_value=18, value=10)

        # Penapisan
        filtered_df = df.copy()

        if kelas_terpilih:
            filtered_df = filtered_df[filtered_df["Kelas"].isin(kelas_terpilih)]

        if bulan != "Semua":
            filtered_df = filtered_df[filtered_df["Tarikh_Lahir"].dt.month == bulan]

        if umur:
            tahun_semasa = datetime.date.today().year
            filtered_df = filtered_df[
                filtered_df["Tarikh_Lahir"].dt.year == tahun_semasa - umur
            ]

        st.success(f"{len(filtered_df)} murid ditemui.")
        st.dataframe(filtered_df.reset_index(drop=True))

        # Muat turun hasil tapisan
        def convert_df(df):
            return df.to_excel(index=False, engine='openpyxl')

        st.download_button(
            "📥 Muat Turun Senarai",
            data=convert_df(filtered_df),
            file_name="murid_tapis.xlsx",
            mime="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
        )
else:
    st.info("Sila muat naik fail Excel untuk mula menapis data.")
import streamlit as st
import pandas as pd
import datetime

st.set_page_config(page_title="Senarai Murid Mengikut Tarikh Lahir & Kelas")
st.title("📋 Penapis Murid Berdasarkan Tarikh Lahir dan Kelas")

# Muat naik fail Excel
uploaded_file = st.file_uploader("Muat naik fail Excel (.xlsx)", type=["xlsx"])

if uploaded_file:
    df = pd.read_excel(uploaded_file)

    # Pastikan kolum betul
    required_columns = {"Nama", "Kelas", "Tarikh_Lahir"}
    if not required_columns.issubset(set(df.columns)):
        st.error("Fail mesti mengandungi kolum: Nama, Kelas, Tarikh_Lahir")
    else:
        # Tukar kolum tarikh kepada format datetime
        df["Tarikh_Lahir"] = pd.to_datetime(df["Tarikh_Lahir"], errors='coerce')
        df = df.dropna(subset=["Tarikh_Lahir"])  # Buang data tanpa tarikh sah

        # Sidebar untuk penapisan
        st.sidebar.header("🔍 Penapis")

        # Pilih kelas
        kelas_terpilih = st.sidebar.multiselect("Pilih Kelas", sorted(df["Kelas"].unique()))

        # Pilih bulan lahir
        bulan = st.sidebar.selectbox("Pilih Bulan Lahir", ["Semua"] + list(range(1, 13)), index=0)

        # Pilih umur
        umur = st.sidebar.slider("Umur (tahun)", min_value=6, max_value=18, value=10)

        # Penapisan
        filtered_df = df.copy()

        if kelas_terpilih:
            filtered_df = filtered_df[filtered_df["Kelas"].isin(kelas_terpilih)]

        if bulan != "Semua":
            filtered_df = filtered_df[filtered_df["Tarikh_Lahir"].dt.month == bulan]

        if umur:
            tahun_semasa = datetime.date.today().year
            filtered_df = filtered_df[
                filtered_df["Tarikh_Lahir"].dt.year == tahun_semasa - umur
            ]

        st.success(f"{len(filtered_df)} murid ditemui.")
        st.dataframe(filtered_df.reset_index(drop=True))

        # Muat turun hasil tapisan
        def convert_df(df):
            return df.to_excel(index=False, engine='openpyxl')

        st.download_button(
            "📥 Muat Turun Senarai",
            data=convert_df(filtered_df),
            file_name="murid_tapis.xlsx",
            mime="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
        )
else:
    st.info("Sila muat naik fail Excel untuk mula menapis data.")
