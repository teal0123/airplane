import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import streamlit as st
import seaborn as sns
import plotly.express as px
import os
st.set_page_config(page_title="AIRPLANE PASSENGERS",page_icon="	:airplane_departure:",layout="wide")
st.title(" :airplane_arriving: AIRPLANE PASENGERS")
st.markdown("<style>div.block-containers{padding-top:1rem;}<style>",unsafe_allow_html=True)
f1=st.file_uploader(":file-folderuploadfile",type=("csv","xls","xlxs"))
if f1 is not None:
    filename=f1.name
    st.write(filename)
    df=pd.read_csv(filename,encoding="iso-8859-1")
else:
    os.chdir(r"C:\Users\HP\Downloads\airplane-passengers")
    df=pd.read_csv("airline-passengers.csv")  
col1,col2=st.columns((2)) 
df["Date"]=pd.to_datetime (df["Date"])
start_date=pd.to_datetime(df["Date"]) .min()
end_date=pd.to_datetime(df["Date"]) .max()
with col1:
    date1=pd.to_datetime(st.date_input("start date",start_date))
with col2:
    date2=pd.to_datetime(st.date_input("end date",end_date))
df=df[(df["Date"]>=date1)&(df["Date"]<=date2)].copy()
#calculate the total passengers
total_passengers = df['total_passengers'].sum()
  # Insert 'year' column derived from 'date' column
df['year'] = df['Date'].dt.year
 # Display DataFrame
st.write("## Passenger Data")
st.write(df)
 # Calculate Total Passengers
total_passengers = df['total_passengers'].sum()
st.write(f"### Total Passengers: {total_passengers}")

 # Group data by year and sum passengers
passengers_per_year = df.groupby('year')['total_passengers'].sum().reset_index()

# Create bar Chart for Total Passengers by Year
st.write("## Passenger Distribution by Year")
fig_year = px.bar(passengers_per_year,x='year', y='total_passengers', title='Passenger Distribution by Year')
st.plotly_chart(fig_year)
