import { Component } from '@angular/core';
import { FormBuilder, FormGroup } from '@angular/forms';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-file-upload-form',
  templateUrl: './file-upload-form.component.html',
  styleUrls: ['./file-upload-form.component.css']
})
export class FileUploadFormComponent {
  form: FormGroup; // Grupo de formulários para armazenar as respostas
  base64Anexo: string | null = null; // Para armazenar o anexo convertido em base64

  constructor(private fb: FormBuilder, private http: HttpClient) {
    // Cria o formulário com FormBuilder
    this.form = this.fb.group({
      name: [''], // Campo de texto exemplo
      email: [''], // Campo de e-mail exemplo
      anexo: ['']  // Campo onde será armazenado o anexo em base64
    });
  }

  // Método chamado quando um arquivo é selecionado
  onAnexoSelected(event: any) {
    const anexo: File = event.target.files[0]; // Obtém o arquivo selecionado

    // Verifica se um arquivo foi selecionado e se é um arquivo .msg
    if (anexo && anexo.name.endsWith('.msg')) {
      console.log('Arquivo válido selecionado:', anexo.name); // Loga o nome do arquivo
      this.convertToBase64(anexo); // Converte o arquivo para base64
    } else {
      console.error('Por favor, selecione um arquivo .msg válido.');
      this.resetFileInput(); // Limpa o campo de arquivo
    }
  }

  // Converte o arquivo para base64 e atualiza o formulário
  convertToBase64(anexo: File) {
    const reader = new FileReader(); // Usa FileReader para ler o arquivo

    // Quando o arquivo for completamente lido
    reader.onload = () => {
      this.base64Anexo = (reader.result as string).split(',')[1]; // Converte para base64
      console.log('Arquivo convertido para base64:', this.base64Anexo); // Loga o conteúdo base64 para verificar
      this.form.patchValue({ anexo: this.base64Anexo }); // Atualiza o campo 'anexo' no formulário com o valor base64
    };

    // Lê o arquivo como Data URL
    reader.readAsDataURL(anexo);
  }

  // Método que é chamado ao submeter o formulário
  onSubmit() {
    if (this.form.valid) {
      // Obtém os valores do formulário e envia ao back-end
      const formData = this.form.value; // Pega os dados do formulário
      
      console.log('Formulário enviado com os seguintes dados:', formData); // Verifica os dados antes de enviar
      
      // Exemplo de requisição HTTP POST para enviar o formulário ao back-end
      this.http.post('URL_DO_SEU_BACKEND', formData)
        .subscribe(response => {
          console.log('Formulário enviado com sucesso:', response); // Sucesso
          this.resetFileInput(); // Limpa o campo de arquivo após o envio bem-sucedido
        }, error => {
          console.error('Erro ao enviar o formulário:', error); // Erro
        });
    } else {
      console.error('O formulário é inválido.');
    }
  }

  // Função para limpar o campo de arquivo (input type="file")
  resetFileInput() {
    const fileInput = document.getElementById('fileInput') as HTMLInputElement;
    if (fileInput) {
      fileInput.value = ''; // Limpa o valor do campo de arquivo
    }
  }
}
