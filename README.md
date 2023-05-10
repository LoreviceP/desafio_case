## Repositório criado para disponibilização dos arquivos do desafio.

Código nos links abaixo atendendo o desafio de extração das tabelas dinâmicas do arquivo "vendas-combustiveis-m3.xls".

- Sales of oil derivative fuels by UF and product
- Sales of diesel by UF and type


### Links
Para uma melhor avaliação optei por disponibilizar o código no Google Colab, conforme link abaixo.

[- Arquivo ".ipynb"](https://github.com/LoreviceP/desafio_case/blob/main/case_data_engineering_test.ipynb)

Segue também o arquivo ".py".

[- Arquivo ".py"](https://github.com/LoreviceP/desafio_case/blob/main/case_data_engineering_test.py)

Arquivos gerados pelo código.

[- Files](https://github.com/LoreviceP/desafio_case/tree/main/files)

### Orquestração de pipeline de dados

O desafio também propôs orquestrar um pipeline de dados.
A solução que eu entendi ser factível foi utilizando o Airflow, contudo ainda não tinha utilizado o mesmo.

Me propus a aprender e por utilizar no meu computador o Windows no momento, tentei de duas maneiras, utilizando o Docker e com um Ubuntu habilitando o subsistema do Windows para Linux.

Pelo docker consegui escrever algumas Dags, segmentando estas em tasks porém obtive alguns erros no momento da utilização de alguns pacotes python.

Já no Ubuntu, pensei em utilizar a Dag para rodar o script como um todo para posteriormente reorganizar em taks.
Uma dificuldade foi fornecer o caminho correto (pois está no windows por trás) para que o airflow conseguisse rodar o pipeline.


### Exemplo de DAG de teste, encontrei em um fórum e estive tentando fazer funcionar.

    from airflow import DAG
    from airflow.operators import BashOperator,PythonOperator
    from datetime import datetime, timedelta

    seven_days_ago = datetime.combine(datetime.today() - timedelta(7),
                                      datetime.min.time())

    default_args = {
        'owner': 'airflow',
        'depends_on_past': False,
        'start_date': seven_days_ago,
        'email': ['airflow@airflow.com'],
        'email_on_failure': False,
        'email_on_retry': False,
        'retries': 1,
        'retry_delay': timedelta(minutes=5),
      }

    dag = DAG('simple', default_args=default_args)
    t1 = BashOperator(
        task_id='testairflow',
        bash_command='python /home/airflow/airflow/dags/scripts/file1.py',
        dag=dag)

Apenas a nível de aprendizado consegui fazer funcionar DAGs mais simples como esta, com parte do código do desafio.

    from airflow import DAG
    from airflow.operators.python import PythonOperator
    from datetime import datetime, timedelta

    def file_request():
        # File request
        file_url = 'http://raw.githubusercontent.com/raizen-analytics/data-engineering-test/master/assets/vendas-combustiveis-m3.xls'
        filename = 'vendas_combustiveis_m3.xls'

        request.urlretrieve(file_url, filename)


    with DAG(dag_id='file_request', schedule=None, start_date=datetime(2023,5,9)) as dag:
        request_file2 = PythonOperator(
            task_id='file_request',
            python_callable=file_request
        )
