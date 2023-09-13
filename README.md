# Admin Sky Blog

Tick & trips for more than 10 years

Info:
import os

root_directory = '/'

def find_md_files(directory):
    md_files = []
    for root, _, files in os.walk(directory):
        for file in files:
            if file.endswith('.md'):
                md_files.append(os.path.join(root, file))
    return md_files

def generate_md_list(md_files):
    md_list = ""
    for file in md_files:
        # Создаем относительный путь от корня проекта
        relative_path = os.path.relpath(file, root_directory)
        # Создаем ссылку на файл
        md_list += f"- [{os.path.basename(relative_path)}]({relative_path})\n"
    return md_list

md_files = find_md_files(root_directory)

markdown_list = generate_md_list(md_files)

with open('project_list.md', 'w') as file:
    file.write(markdown_list)
