## Repositório criado para disponibilização dos arquivos do desafio.

Código para extração das tabelas dinâmicas do arquivo "vendas-combustiveis-m3.xls".

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

Em relação à orquestrar o pipeline de dados, ainda não tinha utilizado o airflow.
Por utilizar o Windows no momento, tentei de duas maneiras, utilizando o Docker e com um Ubuntu emulado.

Pelo docker consegui escrever algumas Dags, segmentando estas em tasks porém obtive alguns erros no momento da utilização de alguns pacotes python.

Já no Ubuntu, por não estar familiarizado, pensei em utilizar a Dag para rodar o script como um todo para posteriormente reorganizar em taks.

Porém uma dificuldade foi fornecer o caminho correto (pois está no windows por trás) para que o airflow conseguisse rodar o pipeline.

### Exemplo de DAG de teste:

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

